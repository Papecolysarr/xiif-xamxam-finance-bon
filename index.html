<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Xiif XamXam Finances - V2</title>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, addDoc, collection, query, orderBy, limit, onSnapshot } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

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

        // --- NAVIGATION ---
        window.showPage = (id) => {
            document.querySelectorAll('.pg').forEach(p => p.classList.remove('on'));
            document.getElementById('pg-' + id).classList.add('on');
            document.querySelectorAll('.ntab').forEach(t => t.classList.remove('on'));
            const activeTab = document.querySelector(`[onclick="showPage('${id}')"]`);
            if(activeTab) activeTab.classList.add('on');
        };

        // --- LOGIQUE TRANSACTION ---
        window.oaTx = (type) => {
            document.getElementById('ov-tx').classList.add('on');
            document.getElementById('txtp').value = type;
            document.getElementById('txdt').valueAsDate = new Date();
            window.upSubOpts();
        };

        window.upSubOpts = () => {
            const plan = document.getElementById('txpl').value;
            const sc = document.getElementById('txsc');
            const cats = {
                conso: ["Loyer", "Nourriture", "Transport", "Factures", "Santé", "Loisirs"],
                don: ["Famille", "Zakat", "Aumône", "Cadeaux"],
                epargne: ["Fond d'urgence", "Projet Immo", "Épargne Long Terme"],
                invest: ["Bourse/Crypto", "Business local", "Formation"],
                dette: ["Remboursement prêt", "Dette amicale"]
            };
            let html = '<option value="Autres">-- Autres (Par défaut) --</option>';
            if(cats[plan]) cats[plan].forEach(c => html += `<option value="${c}">${c}</option>`);
            sc.innerHTML = html;
        };

        window.savTx = async () => {
            const btn = document.getElementById('bstx');
            const data = {
                type: document.getElementById('txtp').value,
                desc: document.getElementById('txds').value || "Sans titre",
                montant: Number(document.getElementById('txam').value),
                date: document.getElementById('txdt').value,
                plan: document.getElementById('txpl').value,
                sousCategorie: document.getElementById('txsc').value,
                createdAt: new Date()
            };
            if(!data.montant) return alert("Indique le montant !");
            btn.disabled = true;
            try {
                await addDoc(collection(db, "transactions"), data);
                document.getElementById('ov-tx').classList.remove('on');
                alert("Enregistré !");
            } catch (e) { alert("Erreur Firebase"); }
            btn.disabled = false;
        };

        // --- AUTH & INITIALISATION ---
        onAuthStateChanged(auth, user => {
            if (user) {
                document.getElementById('ap').style.display = 'flex';
                document.getElementById('ld').style.display = 'none';
            } else {
                // Logique de redirection login si besoin
            }
        });
    </script>

    <style>
        :root {
            --g:#16A34A; --r:#DC2626; --b:#2563EB; --am:#D97706; --w:#FFF;
            --s0:#F9FAFB; --s2:#E5E7EB; --s5:#6B7280; --s8:#1F2937;
            --shm:0 4px 16px rgba(0,0,0,.12); --r20:20px;
        }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--s0); margin: 0; color: var(--s8); }
        
        /* NAVIGATION */
        .tnav { background: var(--w); border-bottom: 1px solid var(--s2); display: flex; align-items: center; padding: 0 15px; position: sticky; top: 0; z-index: 100; height: 60px; }
        .tbrand { font-weight: 800; font-size: 16px; margin-right: 20px; }
        .ttabs { display: flex; gap: 5px; overflow-x: auto; flex: 1; }
        .ntab { padding: 10px 15px; font-size: 13px; font-weight: 600; color: var(--s5); cursor: pointer; border-radius: 8px; white-space: nowrap; }
        .ntab.on { color: var(--g); background: #f0fdf4; }

        /* CONTENU */
        .cnt { padding: 20px; max-width: 800px; margin: 0 auto; }
        .pg { display: none; animation: fadeIn 0.3s; }
        .pg.on { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

        /* ACCUEIL : 4 CASES */
        .home-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 10px; }
        .h-card { padding: 35px 20px; border-radius: var(--r20); color: white; text-align: center; cursor: pointer; transition: 0.2s; box-shadow: var(--shm); border: none; }
        .h-card:hover { transform: scale(1.02); filter: brightness(1.1); }
        .h-ico { font-size: 35px; display: block; margin-bottom: 10px; }
        .h-txt { font-weight: 800; font-size: 14px; text-transform: uppercase; }

        .bg { background: var(--g); } .br { background: var(--r); }
        .bam { background: var(--am); } .bb { background: var(--b); }

        /* MODALS */
        .ov { display:none; position:fixed; inset:0; background:rgba(0,0,0,.6); z-index:500; align-items:center; justify-content:center; backdrop-filter: blur(3px); }
        .ov.on { display:flex; }
        .mo { background:white; padding:25px; border-radius:24px; width:90%; max-width:420px; }
        .fl { margin-bottom: 15px; }
        .fl label { display:block; font-size:11px; font-weight:700; text-transform: uppercase; color: var(--s5); margin-bottom: 6px; }
        .fl input, .fl select { width:100%; padding:12px; border:1.5px solid var(--s2); border-radius:12px; font-size: 15px; box-sizing: border-box; }
        .btn-row { display:flex; gap:10px; margin-top:20px; }
        .btn { flex:1; padding:14px; border:none; border-radius:12px; font-weight:700; cursor:pointer; }
    </style>
</head>
<body>

<div id="ld" style="text-align:center; padding:100px;">Chargement...</div>

<div id="ap" style="display:none; flex-direction:column;">
    <nav class="tnav">
        <div class="tbrand">Xiif XamXam</div>
        <div class="ttabs">
            <div class="ntab on" onclick="showPage('db')">🏠 Accueil</div>
            <div class="ntab" onclick="showPage('tx')">💳 Historique</div>
            <div class="ntab" onclick="showPage('plans')">📊 Plans</div>
            <div class="ntab" onclick="showPage('param')">⚙️ Paramètres</div>
        </div>
    </nav>

    <div class="cnt">
        <div class="pg on" id="pg-db">
            <h2 style="margin-bottom: 20px; font-weight: 800;">Que voulez-vous faire ?</h2>
            <div class="home-grid">
                <button class="h-card bg" onclick="oaTx('revenu')">
                    <span class="h-ico">💰</span><div class="h-txt">Ajouter un Revenu</div>
                </button>
                <button class="h-card br" onclick="oaTx('depense')">
                    <span class="h-ico">💸</span><div class="h-txt">Ajouter une Dépense</div>
                </button>
                <button class="h-card bam" onclick="oaTx('depense'); document.getElementById('txpl').value='dette';">
                    <span class="h-ico">⚠️</span><div class="h-txt">Contracter une Dette</div>
                </button>
                <button class="h-card bb" onclick="document.getElementById('ov-retrait').classList.add('on');">
                    <span class="h-ico">🏦</span><div class="h-txt">Retrait Épargne</div>
                </button>
            </div>
        </div>

        <div class="pg" id="pg-tx">
            <h3>Historique des transactions</h3>
            <p style="color:var(--s5)">Les transactions s'afficheront ici...</p>
        </div>

        <div class="pg" id="pg-plans">
            <h3>Répartition par Plan</h3>
            <p>Ici tes statistiques de dépenses par catégorie.</p>
        </div>
    </div>
</div>

<div class="ov" id="ov-tx">
    <div class="mo">
        <h3 id="m-tit">Nouvelle Opération</h3>
        <input type="hidden" id="txtp">
        <div class="fl"><label>Description</label><input id="txds" placeholder="Ex: Salaire, Loyer..."></div>
        <div class="fl"><label>Montant (FCFA)</label><input id="txam" type="number"></div>
        <div class="fl"><label>Date</label><input id="txdt" type="date"></div>
        <div class="fl">
            <label>Plan</label>
            <select id="txpl" onchange="upSubOpts()">
                <option value="conso">🛒 Consommation</option>
                <option value="don">🤲 Don</option>
                <option value="epargne">💳 Épargne</option>
                <option value="invest">📈 Investissement</option>
                <option value="dette">⚠️ Dette</option>
            </select>
        </div>
        <div class="fl"><label>Sous-catégorie</label><select id="txsc"></select></div>
        <div class="btn-row">
            <button class="btn" style="background:#eee" onclick="document.getElementById('ov-tx').classList.remove('on')">Annuler</button>
            <button class="btn bg" id="bstx" onclick="savTx()" style="color:white">Enregistrer</button>
        </div>
    </div>
</div>

<div class="ov" id="ov-retrait">
    <div class="mo">
        <h3>↩ Retrait Épargne</h3>
        <div class="fl">
            <label>Retirer de quelle caisse ?</label>
            <select id="re-src">
                <option value="urgence">🚨 Fond d'urgence</option>
                <option value="projet">🏗️ Projet Immobilier</option>
                <option value="autre">📦 Autre</option>
            </select>
        </div>
        <div class="fl"><label>Montant</label><input id="re-amt" type="number"></div>
        <div class="fl"><label>Note</label><input id="re-note" placeholder="Pourquoi ?"></div>
        <div class="btn-row">
            <button class="btn" style="background:#eee" onclick="document.getElementById('ov-retrait').classList.remove('on')">Annuler</button>
            <button class="btn bb" style="color:white" onclick="alert('Retrait enregistré !'); document.getElementById('ov-retrait').classList.remove('on')">Confirmer</button>
        </div>
    </div>
</div>

</body>
</html>
