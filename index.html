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
        import { getAuth, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
        import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, getDocs } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

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

        // --- GESTION DES SOUS-CATÉGORIES ---
        window.upSubOpts = () => {
            const plan = document.getElementById('txpl').value;
            const sc = document.getElementById('txsc');
            const cats = {
                conso: ["Nourriture", "Loyer", "Transport", "Santé", "Loisirs"],
                don: ["Famille", "Zakat", "Aumône", "Cadeaux"],
                epargne: ["Urgence", "Projet Immobilier", "Epargne Long Terme"],
                invest: ["Bourse/Crypto", "Business local", "Formation"],
                dette: ["Remboursement prêt", "Dette amicale"]
            };
            let html = '<option value="Autres">-- Autres --</option>';
            if(cats[plan]) cats[plan].forEach(c => html += `<option value="${c}">${c}</option>`);
            sc.innerHTML = html;
        };

        // --- ACTIONS ---
        window.oaTx = (type) => {
            document.getElementById('ov-tx').classList.add('on');
            document.getElementById('txtp').value = type;
            document.getElementById('txdt').valueAsDate = new Date();
            window.upSubOpts();
        };

        window.savTx = async () => {
            const btn = document.getElementById('bstx');
            const data = {
                type: document.getElementById('txtp').value,
                desc: document.getElementById('txds').value || "Sans titre",
                montant: Number(document.getElementById('txam').value),
                date: document.getElementById('txdt').value,
                plan: document.getElementById('txpl').value,
                sousCategorie: document.getElementById('txsc').value || "Autres",
                createdAt: new Date()
            };
            if(!data.montant) return alert("Montant obligatoire !");
            btn.disabled = true;
            try {
                await addDoc(collection(db, "transactions"), data);
                document.getElementById('ov-tx').classList.remove('on');
                document.getElementById('txds').value = ""; document.getElementById('txam').value = "";
            } catch (e) { alert("Erreur d'enregistrement"); }
            btn.disabled = false;
        };

        // --- EXPORT EXCEL ---
        window.exportExcel = async () => {
            const q = query(collection(db, "transactions"), orderBy("date", "desc"));
            const querySnapshot = await getDocs(q);
            const data = [];
            
            querySnapshot.forEach((doc) => {
                const t = doc.data();
                data.push({
                    "Date": t.date,
                    "Type": t.type,
                    "Description": t.desc,
                    "Montant (FCFA)": t.montant,
                    "Plan": t.plan,
                    "Sous-Catégorie": t.sousCategorie
                });
            });

            const worksheet = XLSX.utils.json_to_sheet(data);
            const workbook = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(workbook, worksheet, "Finances");
            XLSX.writeFile(workbook, `Xiif_Finances_${new Date().toISOString().slice(0,10)}.xlsx`);
        };

        // --- CHARGEMENT DES DONNÉES ---
        function loadData() {
            const q = query(collection(db, "transactions"), orderBy("date", "desc"));
            onSnapshot(q, (snapshot) => {
                let txHtml = "";
                let totals = { conso:0, don:0, epargne:0, invest:0, dette:0 };

                snapshot.forEach((doc) => {
                    const t = doc.data();
                    const isRev = t.type === 'revenu';
                    txHtml += `
                        <div class="tx-item" style="border-left:5px solid ${isRev?'var(--g)':'var(--r)'}">
                            <div><strong>${t.desc}</strong><br><small>${t.date} | ${t.sousCategorie}</small></div>
                            <div style="color:${isRev?'var(--g)':'var(--r)'}; font-weight:800">
                                ${isRev?'+':'-'} ${t.montant.toLocaleString()} F
                            </div>
                        </div>`;
                    if(!isRev && totals[t.plan] !== undefined) totals[t.plan] += t.montant;
                });

                document.getElementById('txlist').innerHTML = txHtml || "Aucune transaction.";
                document.getElementById('plan-stats').innerHTML = `
                    <div class="stat-card">🛒 Consommation: <b>${totals.conso.toLocaleString()} F</b></div>
                    <div class="stat-card">🤲 Don: <b>${totals.don.toLocaleString()} F</b></div>
                    <div class="stat-card">💳 Épargne: <b>${totals.epargne.toLocaleString()} F</b></div>
                    <div class="stat-card">📈 Invest: <b>${totals.invest.toLocaleString()} F</b></div>
                `;
            });
        }

        onAuthStateChanged(auth, user => {
            if (user) {
                document.getElementById('ap').style.display = 'flex';
                document.getElementById('ld').style.display = 'none';
                loadData();
            }
        });
    </script>

    <style>
        :root {
            --g:#16A34A; --r:#DC2626; --b:#2563EB; --am:#D97706; --w:#FFF;
            --s0:#F9FAFB; --s2:#E5E7EB; --s5:#6B7280; --s8:#1F2937;
            --shm:0 4px 16px rgba(0,0,0,.12); --r20:20px;
        }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--s0); margin: 0; }
        .tnav { background: var(--w); border-bottom: 1px solid var(--s2); display: flex; align-items: center; padding: 0 15px; height: 60px; position: sticky; top:0; z-index:100; }
        .ttabs { display: flex; gap: 5px; overflow-x: auto; flex: 1; margin-left: 10px; }
        .ntab { padding: 8px 12px; font-size: 13px; font-weight: 600; color: var(--s5); cursor: pointer; border-radius: 8px; white-space: nowrap; }
        .ntab.on { color: var(--g); background: #f0fdf4; }
        .cnt { padding: 20px; max-width: 600px; margin: 0 auto; }
        .pg { display: none; } .pg.on { display: block; }

        .home-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
        .h-card { padding: 30px 10px; border-radius: var(--r20); color: white; text-align: center; cursor: pointer; border: none; box-shadow: var(--shm); font-weight:800; }
        .bg { background: var(--g); } .br { background: var(--r); } .bam { background: var(--am); } .bb { background: var(--b); }
        
        .tx-item { background: white; padding: 12px; border-radius: 12px; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center; border: 1px solid var(--s2); }
        .stat-card { background: white; padding: 15px; border-radius: 12px; margin-bottom: 10px; border-left: 5px solid var(--g); box-shadow: var(--shm); }

        .ov { display:none; position:fixed; inset:0; background:rgba(0,0,0,.6); z-index:500; align-items:center; justify-content:center; backdrop-filter: blur(3px); }
        .ov.on { display:flex; }
        .mo { background:white; padding:25px; border-radius:24px; width:90%; max-width:400px; }
        .fl { margin-bottom: 12px; }
        .fl label { display:block; font-size:11px; font-weight:700; text-transform: uppercase; color: var(--s5); margin-bottom: 4px; }
        .fl input, .fl select { width:100%; padding:10px; border:1.5px solid var(--s2); border-radius:10px; font-size: 14px; box-sizing: border-box; }
        .btn-row { display:flex; gap:10px; margin-top:15px; }
        .btn { flex:1; padding:12px; border:none; border-radius:10px; font-weight:700; cursor:pointer; }
    </style>
</head>
<body>

<div id="ld" style="text-align:center; padding-top:100px;">Initialisation...</div>

<div id="ap" style="display:none; flex-direction:column;">
    <nav class="tnav">
        <div style="font-weight:900; font-size:18px;">Xiif</div>
        <div class="ttabs">
            <div class="ntab on" onclick="showPage('db')">🏠 Accueil</div>
            <div class="ntab" onclick="showPage('tx')">💳 Historique</div>
            <div class="ntab" onclick="showPage('plans')">📊 Plans</div>
            <div class="ntab" onclick="showPage('param')">⚙️ Paramètres</div>
        </div>
    </nav>

    <div class="cnt">
        <div class="pg on" id="pg-db">
            <h2 style="margin-bottom:20px;">Bonjour 👋</h2>
            <div class="home-grid">
                <button class="h-card bg" onclick="oaTx('revenu')">💰 Revenu</button>
                <button class="h-card br" onclick="oaTx('depense')">💸 Dépense</button>
                <button class="h-card bam" onclick="oaTx('depense'); document.getElementById('txpl').value='dette'; window.upSubOpts();">⚠️ Dette</button>
                <button class="h-card bb" onclick="document.getElementById('ov-retrait').classList.add('on');">🏦 Retrait Ep.</button>
            </div>
        </div>

        <div class="pg" id="pg-tx">
            <h3>Dernières opérations</h3>
            <div id="txlist">Chargement...</div>
        </div>

        <div class="pg" id="pg-plans">
            <h3>Dépenses cumulées</h3>
            <div id="plan-stats"></div>
        </div>

        <div class="pg" id="pg-param">
            <h3>Paramètres & Données</h3>
            <div class="stat-card" style="border-left-color: var(--b); cursor: pointer;" onclick="exportExcel()">
                <strong>📥 Exporter mes données (.xlsx)</strong>
                <p style="margin:5px 0 0; font-size:12px; color:var(--s5)">Télécharge toutes tes transactions dans un fichier Excel.</p>
            </div>
            <button class="btn" style="background:var(--r); color:white; width:100%; margin-top:20px;" onclick="auth.signOut()">Se déconnecter</button>
        </div>
    </div>
</div>

<div class="ov" id="ov-tx">
    <div class="mo">
        <h3>Nouvelle Opération</h3>
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
            <label>Caisse source</label>
            <select id="re-src">
                <option value="urgence">🚨 Fond d'urgence</option>
                <option value="projet">🏗️ Projet</option>
            </select>
        </div>
        <div class="fl"><label>Montant</label><input type="number" id="re-amt"></div>
        <div class="btn-row">
            <button class="btn" style="background:#eee" onclick="document.getElementById('ov-retrait').classList.remove('on')">Annuler</button>
            <button class="btn bb" style="color:white" onclick="alert('Retrait simulé !'); document.getElementById('ov-retrait').classList.remove('on')">Valider</button>
        </div>
    </div>
</div>

</body>
</html>
