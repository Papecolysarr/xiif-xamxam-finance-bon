<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Xiif XamXam - Pro</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getAuth, onAuthStateChanged, signOut, signInWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
import { getFirestore, doc, setDoc, getDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig = {
    apiKey: "AIzaSyAPw7tRUftioT_Jap0iupmeSH-jsJdBp5w",
    authDomain: "xiif-xamxam.firebaseapp.com",
    projectId: "xiif-xamxam",
    storageBucket: "xiif-xamxam.firebasestorage.app",
    messagingSenderId: "518564223931",
    appId: "1:518564223931:web:0881991192a0cc0003429e"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

let currentUser = null;
let userData = { 
    transactions: [], 
    config: { plans: { don: 10, epargne: 10, invest: 5, dette: 10, conso: 65 } } 
};

const SUB_CATS = {
    don: ["Famille", "Zakat", "Aumône", "Cadeaux", "Autres"],
    epargne: ["Fonds d'urgence", "Projet", "Vacances", "Autres"],
    invest: ["Business", "Bourse", "Crypto", "Immobilier", "Autres"],
    dette: ["Banque", "Ami", "Boutique", "Autres"],
    conso: ["Loyer", "Nourriture", "Transport", "Factures", "Santé", "Loisirs", "Autres"]
};

onAuthStateChanged(auth, user => {
    if (user) {
        currentUser = user;
        document.getElementById('ld').style.display = 'none';
        document.getElementById('au').style.display = 'none';
        document.getElementById('ap').style.display = 'flex';
        onSnapshot(doc(db, "users", user.uid), d => { 
            if(d.exists()){ 
                userData = d.data(); 
                renderAll(); 
                fillSettings();
            } 
        });
    } else {
        document.getElementById('ld').style.display = 'none';
        document.getElementById('au').style.display = 'flex';
    }
});

// --- SAUVEGARDE ET REPARTITION ---
window.savTx = async () => {
    const type = document.getElementById('txtp').value;
    const amt = parseFloat(document.getElementById('txam').value);
    const desc = document.getElementById('txds').value;
    const date = document.getElementById('txdt').value;
    if(!amt || !desc) return alert("Champs vides");

    if(type === 'revenu') {
        const p = userData.config.plans;
        const dist = [
            { p: 'don', v: (amt * p.don / 100) },
            { p: 'epargne', v: (amt * p.epargne / 100) },
            { p: 'invest', v: (amt * p.invest / 100) },
            { p: 'dette', v: (amt * p.dette / 100) },
            { p: 'conso', v: (amt * p.conso / 100) }
        ];
        dist.forEach(d => {
            userData.transactions.push({ id:Date.now()+Math.random(), desc: `Rép: ${desc}`, amt: d.v, type: 'revenu', plan: d.p, sub: 'Auto', date });
        });
    } else {
        userData.transactions.push({ 
            id:Date.now(), desc, amt, type: 'depense', 
            plan: document.getElementById('txpl').value, 
            sub: document.getElementById('txsc').value || "Autres", date 
        });
    }
    await setDoc(doc(db, "users", currentUser.uid), userData);
    window.cov('ov-tx');
};

// --- PARAMETRES ---
function fillSettings() {
    const p = userData.config.plans;
    document.getElementById('cfg-conso').value = p.conso;
    document.getElementById('cfg-epargne').value = p.epargne;
    document.getElementById('cfg-invest').value = p.invest;
    document.getElementById('cfg-don').value = p.don;
    document.getElementById('cfg-dette').value = p.dette;
}

window.saveSettings = async () => {
    const c = parseFloat(document.getElementById('cfg-conso').value);
    const e = parseFloat(document.getElementById('cfg-epargne').value);
    const i = parseFloat(document.getElementById('cfg-invest').value);
    const dn = parseFloat(document.getElementById('cfg-don').value);
    const dt = parseFloat(document.getElementById('cfg-dette').value);

    if((c+e+i+dn+dt) !== 100) return alert("Le total doit faire exactement 100% ! Actuellement : " + (c+e+i+dn+dt) + "%");

    userData.config.plans = { conso:c, epargne:e, invest:i, don:dn, dette:dt };
    await setDoc(doc(db, "users", currentUser.uid), userData);
    alert("Paramètres enregistrés !");
    window.gp('db');
};

// --- EXPORT EXCEL ---
window.exportExcel = (months) => {
    const now = new Date();
    const limitDate = new Date();
    limitDate.setMonth(now.getMonth() - months);

    const filtered = userData.transactions.filter(t => new Date(t.date) >= limitDate);
    
    const data = filtered.map(t => ({
        Date: t.date,
        Description: t.desc,
        Type: t.type === 'revenu' ? 'REVENU' : 'DEPENSE',
        Plan: t.plan.toUpperCase(),
        SousCategorie: t.sub,
        Montant_FCFA: t.amt
    }));

    const ws = XLSX.utils.json_to_sheet(data);
    const wb = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(wb, ws, "Rapport_Xiif");
    XLSX.writeFile(wb, `Rapport_${months}mois_XiifXamXam.xlsx`);
};

window.renderAll = () => {
    const txs = userData.transactions || [];
    const plans = ['conso', 'epargne', 'invest', 'don', 'dette'];
    const icons = { don:'🤲', epargne:'💳', invest:'📈', dette:'⚠️', conso:'🛒' };
    
    let htmlPlans = "";
    plans.forEach(p => {
        const inAmt = txs.filter(t => t.plan === p && t.type === 'revenu').reduce((a,b)=>a+b.amt,0);
        const outAmt = txs.filter(t => t.plan === p && t.type === 'depense').reduce((a,b)=>a+b.amt,0);
        const solde = inAmt - outAmt;
        htmlPlans += `<div class="card"><div class="kl">${icons[p]} Plan ${p}</div><div class="kv ${solde < 0 ? 'tr':'tg'}">${solde.toLocaleString()} F</div></div>`;
    });
    document.getElementById('plan-grid').innerHTML = htmlPlans;

    document.getElementById('dbrec').innerHTML = txs.slice(-5).reverse().map(t => `
        <div class="txi">
            <div class="txinf"><div class="fb tsm">${t.desc}</div><div class="txmeta">${t.plan} > ${t.sub}</div></div>
            <div class="${t.type==='revenu'?'tg':'tr'} fb">${t.amt.toLocaleString()}</div>
        </div>
    `).join('');
};

window.gp = (p) => {
    document.querySelectorAll('.pg').forEach(pg => pg.classList.remove('on'));
    document.getElementById('pg-'+p).classList.add('on');
};
window.oaTx = (type) => { 
    document.getElementById('txtp').value = type; 
    document.getElementById('txdt').valueAsDate = new Date();
    const isRev = type === 'revenu';
    document.getElementById('txpf').style.display = isRev ? 'none' : 'block';
    window.upSubOpts();
    document.getElementById('ov-tx').classList.add('on'); 
};
window.upSubOpts = () => {
    const plan = document.getElementById('txpl').value;
    const sc = document.getElementById('txsc'); sc.innerHTML = "";
    (SUB_CATS[plan] || []).forEach(c => {
        let o = document.createElement('option'); o.value = c; o.innerText = c;
        if(c === "Autres") o.selected = true;
        sc.appendChild(o);
    });
};
window.cov = (id) => document.getElementById(id).classList.remove('on');
</script>

<style>
:root{ --g:#16A34A; --r:#DC2626; --b:#2563EB; --am:#D97706; --s0:#F9FAFB; --s2:#E5E7EB; --s5:#6B7280; --s8:#1F2937; --w:#FFF; --font:'Plus Jakarta Sans',sans-serif; }
body{ font-family:var(--font); background:var(--s0); color:var(--s8); margin:0; }
.tnav{ background:var(--w); padding:15px 20px; border-bottom:1px solid var(--s2); display:flex; justify-content:space-between; align-items:center; position:sticky; top:0; z-index:50;}
.cnt{ padding:20px; max-width:1100px; margin:0 auto; }
.card{ background:var(--w); padding:15px; border-radius:12px; border:1px solid var(--s2); box-shadow:0 1px 3px rgba(0,0,0,0.05); margin-bottom:10px; }
.grid4{ display:grid; grid-template-columns:repeat(auto-fit, minmax(140px, 1fr)); gap:12px; margin-bottom:25px; }
.grid-plans{ display:grid; grid-template-columns:repeat(auto-fit, minmax(180px, 1fr)); gap:12px; margin-bottom:25px; }
.kl{ font-size:11px; font-weight:800; color:var(--s5); text-transform:uppercase; margin-bottom:5px; }
.kv{ font-size:18px; font-weight:800; }
.fb{ font-weight:700; } .tsm{ font-size:13px; } .tmu{ color:var(--s5); } .tg{ color:var(--g); } .tr{ color:var(--r); }
.ov{ display:none; position:fixed; inset:0; background:rgba(0,0,0,0.5); z-index:100; align-items:center; justify-content:center; }
.ov.on{ display:flex; }
.mo{ background:var(--w); padding:25px; border-radius:16px; width:90%; max-width:400px; }
.fl{ margin-bottom:12px; } .fl label{ display:block; font-size:10px; font-weight:800; margin-bottom:4px; }
.fl input, .fl select{ width:100%; padding:10px; border:1px solid var(--s2); border-radius:8px; }
.btn{ padding:12px; border-radius:8px; border:none; font-weight:700; cursor:pointer; width:100%; margin-top:5px;}
.bg{ background:var(--g); color:var(--w); } .bo{ background:var(--s2); color:var(--s8); } .bb{ background:var(--b); color:var(--w); }
.txi{ display:flex; justify-content:space-between; padding:10px 0; border-bottom:1px solid var(--s0); }
.pg{ display:none; } .pg.on{ display:block; }
.ex-row{ display:flex; gap:10px; margin-top:10px; }
</style>
</head>
<body>

<div id="ld" style="text-align:center; padding:100px;">Chargement...</div>

<div id="au" style="display:none; align-items:center; justify-content:center; height:100vh;">
    <div class="mo card" style="text-align:center">
        <h2>Xiif XamXam</h2>
        <input id="le" type="email" placeholder="Email" class="fl" style="width:90%">
        <input id="lp" type="password" placeholder="Pass" class="fl" style="width:90%">
        <button class="btn bg" onclick="signInWithEmailAndPassword(auth, document.getElementById('le').value, document.getElementById('lp').value)">Connexion</button>
    </div>
</div>

<div id="ap" style="display:none; flex-direction:column;">
    <nav class="tnav">
        <b onclick="gp('db')" style="cursor:pointer">Xiif XamXam 💰</b>
        <button onclick="gp('cfg')" class="btn bo" style="width:auto; padding:5px 15px">⚙️ Paramètres</button>
    </nav>

    <div class="cnt">
        <div class="pg on" id="pg-db">
            <div class="grid4">
                <div class="card" style="border-top:4px solid var(--g); cursor:pointer" onclick="oaTx('revenu')">💰 Revenu</div>
                <div class="card" style="border-top:4px solid var(--r); cursor:pointer" onclick="oaTx('depense')">💸 Dépense</div>
                <div class="card" style="border-top:4px solid var(--am); cursor:pointer" onclick="gp('db')">⚠️ Dettes</div>
                <div class="card" style="border-top:4px solid var(--b); cursor:pointer" onclick="gp('rep')">📊 Rapports</div>
            </div>

            <h3 class="kl">Vos Plans</h3>
            <div class="grid-plans" id="plan-grid"></div>

            <div class="card">
                <div class="fb tsm">Dernières Activités</div>
                <div id="dbrec"></div>
            </div>
        </div>

        <div class="pg" id="pg-cfg">
            <h2 style="margin-bottom:20px;">⚙️ Ajuster vos Pourcentages</h2>
            <div class="card">
                <div class="fl"><label>Consommation (%)</label><input type="number" id="cfg-conso"></div>
                <div class="fl"><label>Épargne (%)</label><input type="number" id="cfg-epargne"></div>
                <div class="fl"><label>Investissement (%)</label><input type="number" id="cfg-invest"></div>
                <div class="fl"><label>Don (%)</label><input type="number" id="cfg-don"></div>
                <div class="fl"><label>Dette (%)</label><input type="number" id="cfg-dette"></div>
                <p class="tsm tmu">Total doit être = 100%</p>
                <button class="btn bg" onclick="saveSettings()">Enregistrer les changements</button>
                <button class="btn bo" onclick="gp('db')">Retour</button>
            </div>
        </div>

        <div class="pg" id="pg-rep">
            <h2>📊 Rapports & Exports</h2>
            <div class="card">
                <p class="tsm tmu" style="margin-bottom:15px">Téléchargez votre historique complet sous format Excel.</p>
                <div class="ex-row">
                    <button class="btn bb" onclick="exportExcel(1)">1 Mois</button>
                    <button class="btn bb" onclick="exportExcel(3)">3 Mois</button>
                    <button class="btn bb" onclick="exportExcel(6)">6 Mois</button>
                </div>
                <button class="btn bo" style="margin-top:20px" onclick="gp('db')">Retour</button>
            </div>
        </div>
    </div>
</div>

<div class="ov" id="ov-tx">
    <div class="mo">
        <h3 id="m-tit">Nouvelle saisie</h3><br>
        <input type="hidden" id="txtp">
        <div class="fl"><label>Description</label><input id="txds" placeholder="Ex: Salaire ou Loyer"></div>
        <div class="fl"><label>Montant (FCFA)</label><input id="txam" type="number"></div>
        <div class="fl"><label>Date</label><input id="txdt" type="date"></div>
        
        <div id="txpf">
            <div class="fl"><label>Plan ciblé</label>
                <select id="txpl" onchange="upSubOpts()">
                    <option value="conso">🛒 Consommation</option>
                    <option value="epargne">💳 Épargne</option>
                    <option value="don">🤲 Don</option>
                    <option value="dette">⚠️ Dette</option>
                    <option value="invest">📈 Invest</option>
                </select>
            </div>
            <div class="fl"><label>Sous-catégorie</label><select id="txsc"></select></div>
        </div>
        <button class="btn bg" onclick="savTx()">Confirmer</button>
        <button class="btn bo" onclick="cov('ov-tx')">Annuler</button>
    </div>
</div>

</body>
</html>
