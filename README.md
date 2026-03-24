<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Xiif XamXam Finances</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getAuth, onAuthStateChanged, signOut, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
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

// Variables Globales
let currentUser = null;
let userData = { transactions: [], dettes: [], config: { plans: { don: 10, epargne: 10, invest: 5, dette: 10, conso: 65 } } };

const SUB_CATS = {
    don: ["Famille", "Zakat", "Aumône", "Cadeaux", "Autres"],
    epargne: ["Fonds d'urgence", "Projet/Achat", "Vacances", "Éducation", "Autres"],
    invest: ["Bourse/Actions", "Crypto", "Business", "Immobilier", "Autres"],
    dette: ["Remboursement prêt", "Dette ami", "Autres"],
    conso: ["Loyer", "Nourriture", "Transport", "Factures", "Santé", "Loisirs", "Autres"]
};

// --- AUTHENTIFICATION ---
onAuthStateChanged(auth, async (user) => {
    if (user) {
        currentUser = user;
        document.getElementById('ld').style.display = 'none';
        document.getElementById('au').style.display = 'none';
        document.getElementById('ap').classList.add('on');
        document.getElementById('unm').innerText = user.displayName || user.email.split('@')[0];
        
        // Ecoute en temps réel des données
        onSnapshot(doc(db, "users", user.uid), (doc) => {
            if (doc.exists()) {
                userData = doc.data();
                renderAll();
            } else {
                saveData(); // Initialise si vide
            }
        });
    } else {
        document.getElementById('ld').style.display = 'none';
        document.getElementById('au').style.display = 'flex';
        document.getElementById('ap').classList.remove('on');
    }
});

// --- ACTIONS PRINCIPALES ---
window.oaTx = (type) => {
    document.getElementById('txtp').value = type;
    document.getElementById('txdt').valueAsDate = new Date();
    document.getElementById('ov-tx').classList.add('on');
    upPlanOpts();
};

window.oaRetrait = () => {
    document.getElementById('ov-retrait').classList.add('on');
};

window.oaNouvDette = () => {
    document.getElementById('nd-date').valueAsDate = new Date();
    document.getElementById('ov-nd').classList.add('on');
};

window.upPlanOpts = () => {
    const type = document.getElementById('txtp').value;
    const pf = document.getElementById('txpf');
    pf.style.display = (type === 'revenu') ? 'none' : 'block';
    upSubOpts();
};

window.upSubOpts = () => {
    const plan = document.getElementById('txpl').value;
    const sc = document.getElementById('txsc');
    sc.innerHTML = "";
    const list = (document.getElementById('txtp').value === 'revenu') ? ["Salaire", "Business", "Cadeau", "Autres"] : SUB_CATS[plan];
    list.forEach(item => {
        let opt = document.createElement('option');
        opt.value = item; opt.innerText = item;
        if(item === "Autres") opt.selected = true;
        sc.appendChild(opt);
    });
};

window.savTx = async () => {
    const desc = document.getElementById('txds').value;
    const amt = parseFloat(document.getElementById('txam').value);
    const type = document.getElementById('txtp').value;
    if(!desc || !amt) return alert("Remplissez les champs");

    const newTx = {
        id: Date.now(),
        desc,
        amt,
        type,
        date: document.getElementById('txdt').value,
        plan: type === 'revenu' ? 'general' : document.getElementById('txpl').value,
        sub: document.getElementById('txsc').value || "Autres"
    };

    userData.transactions.push(newTx);
    await saveData();
    cov('ov-tx');
};

window.execRetrait = async () => {
    const amt = parseFloat(document.getElementById('ret-am').value);
    const caisse = document.getElementById('ret-cs').value;
    if(!amt) return;

    const newTx = {
        id: Date.now(),
        desc: `Retrait Épargne (${caisse})`,
        amt: amt,
        type: 'depense',
        date: new Date().toISOString().split('T')[0],
        plan: 'epargne',
        sub: caisse,
        isRetrait: true
    };

    userData.transactions.push(newTx);
    await saveData();
    cov('ov-retrait');
};

async function saveData() {
    if(currentUser) await setDoc(doc(db, "users", currentUser.uid), userData);
}

window.renderAll = () => {
    const txs = userData.transactions || [];
    const rev = txs.filter(t => t.type === 'revenu').reduce((a, b) => a + b.amt, 0);
    const dep = txs.filter(t => t.type === 'depense').reduce((a, b) => a + b.amt, 0);
    
    document.getElementById('dbkpi').innerHTML = `
        <div class="kpi"><div class="kl">Revenus</div><div class="kv tg">${rev.toLocaleString()} F</div></div>
        <div class="kpi"><div class="kl">Dépenses</div><div class="kv tr">${dep.toLocaleString()} F</div></div>
        <div class="kpi"><div class="kl">Solde</div><div class="kv tb">${(rev - dep).toLocaleString()} F</div></div>
    `;

    const rec = document.getElementById('dbrec');
    rec.innerHTML = txs.slice(-5).reverse().map(t => `
        <div class="txi">
            <div class="txd ${t.type === 'revenu' ? 'rv' : 'dp'}"></div>
            <div class="txinf"><div class="txdesc">${t.desc}</div><div class="txmeta">${t.sub} • ${t.date}</div></div>
            <div class="txamt ${t.type === 'revenu' ? 'p' : 'n'}">${t.type === 'revenu' ? '+' : '-'}${t.amt.toLocaleString()} F</div>
        </div>
    `).join('') || '<p class="est">Aucune activité</p>';
};

window.gp = (p) => {
    document.querySelectorAll('.pg').forEach(pg => pg.classList.remove('on'));
    document.getElementById('pg-'+p).classList.add('on');
    document.querySelectorAll('.sni').forEach(s => s.classList.toggle('on', s.dataset.p === p));
};

window.cov = (id) => document.getElementById(id).classList.remove('on');
window.doLogin = async () => {
    try { await signInWithEmailAndPassword(auth, document.getElementById('le').value, document.getElementById('lp').value); }
    catch(e) { alert("Erreur d'accès"); }
};
window.doLogout = () => signOut(auth);

</script>

<style>
:root{
    --g:#16A34A; --r:#DC2626; --b:#2563EB; --am:#D97706; --s0:#F9FAFB; --s2:#E5E7EB; --s5:#6B7280; --s8:#1F2937; --w:#FFF;
    --sh:0 1px 3px rgba(0,0,0,.08); --r12:12px; --r8:8px; --font:'Plus Jakarta Sans',sans-serif;
}
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:var(--font);background:var(--s0);color:var(--s8);min-height:100vh;}
#ld{display:flex;align-items:center;justify-content:center;height:100vh;background:var(--w);}
#au{display:none;height:100vh;align-items:center;justify-content:center;padding:20px;}
#ap{display:none;flex-direction:column;min-height:100vh;}
#ap.on{display:flex;}
.tnav{background:var(--w);border-bottom:1px solid var(--s2);display:flex;align-items:center;padding:12px 16px;position:sticky;top:0;z-index:100;}
.abody{display:flex;flex:1;}
.sb{width:240px;background:var(--w);border-right:1px solid var(--s2);padding:20px 10px;}
.cnt{flex:1;padding:25px;max-width:1000px;margin:0 auto;}
.pg{display:none;}.pg.on{display:block;}
.card{background:var(--w);border:1px solid var(--s2);border-radius:var(--r12);padding:18px;box-shadow:var(--sh);cursor:pointer;transition:0.2s;}
.card:hover{transform:translateY(-2px);box-shadow:0 4px 12px rgba(0,0,0,0.1);}
.kgrid{display:grid;grid-template-columns:repeat(3,1fr);gap:15px;margin-bottom:25px;}
.kpi{background:var(--w);border:1px solid var(--s2);padding:15px;border-radius:var(--r12);}
.kl{font-size:12px;color:var(--s5);font-weight:700;text-transform:uppercase;margin-bottom:5px;}
.kv{font-size:20px;font-weight:800;}
.txi{display:flex;align-items:center;gap:12px;padding:12px;border-bottom:1px solid var(--s0);}
.txd{width:10px;height:10px;border-radius:50%;}
.txd.rv{background:var(--g);}.txd.dp{background:var(--r);}
.txinf{flex:1;}.txdesc{font-weight:600;font-size:14px;}.txmeta{font-size:12px;color:var(--s5);}
.txamt{font-weight:700;}.p{color:var(--g);}.n{color:var(--r);}
.ov{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.4);z-index:1000;align-items:center;justify-content:center;padding:20px;backdrop-filter:blur(2px);}
.ov.on{display:flex;}
.mo{background:var(--w);padding:25px;border-radius:20px;width:100%;max-width:400px;}
.fl{margin-bottom:15px;}.fl label{display:block;font-size:11px;font-weight:800;margin-bottom:5px;text-transform:uppercase;}
.fl input, .fl select{width:100%;padding:10px;border:1px solid var(--s2);border-radius:8px;outline:none;}
.btn{padding:10px 18px;border-radius:8px;border:none;font-weight:700;cursor:pointer;}
.bg{background:var(--g);color:var(--w);}.br{background:var(--r);color:var(--w);}.bo{background:var(--s2);}
.sni{padding:10px;border-radius:8px;cursor:pointer;font-weight:600;margin-bottom:5px;}
.sni.on{background:var(--s0);color:var(--g);}
</style>
</head>
<body>

<div id="ld">Chargement...</div>

<div id="au">
    <div style="width:100%;max-width:350px;">
        <h1 style="margin-bottom:20px;">Xiif XamXam</h1>
        <div class="fl"><label>Email</label><input id="le" type="email"></div>
        <div class="fl"><label>Mot de passe</label><input id="lp" type="password"></div>
        <button class="btn bg" style="width:100%" onclick="doLogin()">Se connecter</button>
    </div>
</div>

<div id="ap">
    <nav class="tnav">
        <b style="font-size:18px;">Xiif XamXam</b>
        <div style="margin-left:auto; display:flex; align-items:center; gap:10px;">
            <span id="unm" style="font-weight:700;">...</span>
            <button class="btn bo" onclick="doLogout()">Déconnexion</button>
        </div>
    </nav>

    <div class="abody">
        <aside class="sb">
            <div class="sni on" data-p="db" onclick="gp('db')">🏠 Accueil</div>
            <div class="sni" data-p="tx" onclick="gp('tx')">💳 Transactions</div>
            <div class="sni" data-p="dettes" onclick="gp('dettes')">⚠️ Dettes</div>
            <div class="sni" data-p="param" onclick="gp('param')">⚙️ Paramètres</div>
        </aside>

        <main class="cnt">
            <div class="pg on" id="pg-db">
                <div style="margin-bottom:25px;">
                    <h2 style="font-weight:800;">Tableau de Bord</h2>
                    <p style="color:var(--s5)">Gérez vos flux en un clic.</p>
                </div>

                <div style="display:grid; grid-template-columns:repeat(auto-fit, minmax(150px, 1fr)); gap:12px; margin-bottom:30px;">
                    <div class="card" style="border-top:4px solid var(--g)" onclick="oaTx('revenu')">
                        <div style="font-size:24px;">💰</div><div style="margin-top:8px;font-weight:700;font-size:13px;">Revenu</div>
                    </div>
                    <div class="card" style="border-top:4px solid var(--r)" onclick="oaTx('depense')">
                        <div style="font-size:24px;">💸</div><div style="margin-top:8px;font-weight:700;font-size:13px;">Dépense</div>
                    </div>
                    <div class="card" style="border-top:4px solid var(--am)" onclick="oaNouvDette()">
                        <div style="font-size:24px;">⚠️</div><div style="margin-top:8px;font-weight:700;font-size:13px;">Dette</div>
                    </div>
                    <div class="card" style="border-top:4px solid var(--b)" onclick="oaRetrait()">
                        <div style="font-size:24px;">↩️</div><div style="margin-top:8px;font-weight:700;font-size:13px;">Retrait Épargne</div>
                    </div>
                </div>

                <div class="kgrid" id="dbkpi"></div>
                <div class="card" style="cursor:default;">
                    <h4 style="margin-bottom:15px;">Activités récentes</h4>
                    <div id="dbrec"></div>
                </div>
            </div>

            <div class="pg" id="pg-tx">
                <h2>Toutes les transactions</h2>
                <div id="txlist" class="mt4"></div>
            </div>
        </main>
    </div>
</div>

<div class="ov" id="ov-tx">
    <div class="mo">
        <h3>Nouvelle Transaction</h3><br>
        <div class="fl"><label>Type</label><select id="txtp" onchange="upPlanOpts()"><option value="revenu">Revenu</option><option value="depense">Dépense</option></select></div>
        <div class="fl"><label>Description</label><input id="txds" placeholder="Ex: Salaire, Uber..."></div>
        <div class="fl"><label>Montant (FCFA)</label><input id="txam" type="number"></div>
        <div class="fl"><label>Date</label><input id="txdt" type="date"></div>
        <div class="fl" id="txpf"><label>Plan</label><select id="txpl" onchange="upSubOpts()"><option value="don">🤲 Don</option><option value="epargne">💳 Épargne</option><option value="invest">📈 Invest</option><option value="dette">⚠️ Dette</option><option value="conso">🛒 Consommation</option></select></div>
        <div class="fl"><label>Sous-catégorie</label><select id="txsc"></select></div>
        <div style="display:flex; gap:10px; margin-top:20px;">
            <button class="btn bo" style="flex:1" onclick="cov('ov-tx')">Annuler</button>
            <button class="btn bg" style="flex:1" onclick="savTx()">Enregistrer</button>
        </div>
    </div>
</div>

<div class="ov" id="ov-retrait">
    <div class="mo">
        <h3>↩️ Retrait Épargne</h3><br>
        <div class="fl"><label>De quelle caisse ?</label><select id="ret-cs"><option value="Fonds d'urgence">Fonds d'urgence</option><option value="Projet/Achat">Projet/Achat</option><option value="Vacances">Vacances</option><option value="Autres">Autres</option></select></div>
        <div class="fl"><label>Montant</label><input id="ret-am" type="number"></div>
        <button class="btn bg" style="width:100%" onclick="execRetrait()">Confirmer le retrait</button><br><br>
        <button class="btn bo" style="width:100%" onclick="cov('ov-retrait')">Annuler</button>
    </div>
</div>

</body>
</html>
