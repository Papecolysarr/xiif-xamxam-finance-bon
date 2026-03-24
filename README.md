<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Xiif XamXam Finances V2</title>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
        import { getAuth, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
        import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

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

        // --- ÉTAT GLOBAL ---
        let currentTab = 'depense';

        // --- NAVIGATION ---
        window.switchTab = (tab) => {
            currentTab = tab;
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('btn-' + tab).classList.add('active');
            renderLists();
        };

        window.showOverlay = (id) => document.getElementById(id).classList.add('on');
        window.hideOverlay = (id) => document.getElementById(id).classList.remove('on');

        // --- LOGIQUE DASHBOARD & LISTES ---
        let allTransactions = [];

        function updateDashboard() {
            const today = new Date().toISOString().split('T')[0];
            const currentMonth = new Date().getMonth();
            const currentYear = new Date().getFullYear();

            let solde = 0, revDay = 0, depDay = 0, revMonth = 0, depMonth = 0;

            allTransactions.forEach(t => {
                const tDate = new Date(t.date);
                const isToday = t.date === today;
                const isThisMonth = tDate.getMonth() === currentMonth && tDate.getFullYear() === currentYear;

                if (t.type === 'revenu') {
                    solde += t.montant;
                    if (isToday) revDay += t.montant;
                    if (isThisMonth) revMonth += t.montant;
                } else {
                    solde -= t.montant;
                    if (isToday) depDay += t.montant;
                    if (isThisMonth) depMonth += t.montant;
                }
            });

            document.getElementById('db-solde').textContent = solde.toLocaleString() + " F";
            document.getElementById('db-rev-day').textContent = "+" + revDay.toLocaleString();
            document.getElementById('db-dep-day').textContent = "-" + depDay.toLocaleString();
            document.getElementById('db-rev-month').textContent = revMonth.toLocaleString() + " F";
            document.getElementById('db-solde-month').textContent = (revMonth - depMonth).toLocaleString() + " F";
        }

        function renderLists() {
            const list = document.getElementById('tx-list');
            const filtered = allTransactions.filter(t => t.type === currentTab);
            
            list.innerHTML = filtered.map(t => `
                <div class="tx-card">
                    <div class="tx-info">
                        <span class="tx-title">${t.desc}</span>
                        <span class="tx-sub">${t.date} • ${t.sousCategorie}</span>
                    </div>
                    <div class="tx-amt ${t.type}">${t.type==='revenu'?'+':'-'} ${t.montant.toLocaleString()}</div>
                </div>
            `).join('') || `<p style="text-align:center;color:#999;padding:20px;">Aucune donnée en ${currentTab}</p>`;
        }

        // --- AJOUT SÉCURISÉ ---
        window.saveTx = async () => {
            const btn = document.getElementById('btn-save');
            const desc = document.getElementById('in-desc').value;
            const montant = Number(document.getElementById('in-amt').value);
            const date = document.getElementById('in-date').value;
            const plan = document.getElementById('in-plan').value;

            if(!montant || !desc) return alert("Champs vides !");

            // Anti-doublon : désactiver le bouton
            btn.disabled = true;
            btn.innerHTML = "Enregistrement...";

            try {
                await addDoc(collection(db, "transactions"), {
                    type: currentTab,
                    desc, montant, date, plan,
                    sousCategorie: "Autres",
                    createdAt: serverTimestamp()
                });
                
                hideOverlay('ov-add');
                showToast("Élément ajouté avec succès !");
                // Reset form
                document.getElementById('in-desc').value = "";
                document.getElementById('in-amt').value = "";
            } catch (e) {
                alert("Erreur");
            } finally {
                btn.disabled = false;
                btn.innerHTML = "Confirmer l'ajout";
            }
        };

        function showToast(msg) {
            const t = document.getElementById('toast');
            t.textContent = msg;
            t.classList.add('on');
            setTimeout(() => t.classList.remove('on'), 3000);
        }

        // --- SYNC FIREBASE ---
        onAuthStateChanged(auth, user => {
            if (user) {
                document.getElementById('app').style.display = 'block';
                const q = query(collection(db, "transactions"), orderBy("date", "desc"));
                onSnapshot(q, (snap) => {
                    allTransactions = snap.docs.map(d => ({id: d.id, ...d.data()}));
                    updateDashboard();
                    renderLists();
                });
            }
        });
    </script>

    <style>
        :root { --g: #16A34A; --r: #DC2626; --s8: #1F2937; --s2: #E5E7EB; --font: 'Plus Jakarta Sans', sans-serif; }
        body { font-family: var(--font); background: #F9FAFB; margin: 0; color: var(--s8); padding-bottom: 100px; }
        
        /* Dashboard Styles */
        .header { background: white; padding: 25px 20px; border-bottom: 1px solid var(--s2); }
        .solde-label { font-size: 13px; font-weight: 600; color: #666; text-transform: uppercase; }
        .solde-val { font-size: 32px; font-weight: 800; margin: 5px 0 20px; color: #000; }
        
        .dash-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
        .dash-card { background: #F3F4F6; padding: 15px; border-radius: 16px; }
        .dash-card.green { background: #F0FDF4; color: var(--g); }
        .dash-card.red { background: #FEF2F2; color: var(--r); }
        .dash-num { font-weight: 800; font-size: 18px; display: block; }
        .dash-sub { font-size: 11px; font-weight: 600; text-transform: uppercase; opacity: 0.8; }

        /* Navigation Tabs */
        .tabs { display: flex; background: white; padding: 10px 20px; sticky top:0; gap: 10px; border-bottom: 1px solid var(--s2); }
        .tab-btn { flex: 1; padding: 12px; border-radius: 12px; border: none; font-weight: 700; cursor: pointer; background: transparent; color: #999; transition: 0.3s; }
        .tab-btn.active { background: #F3F4F6; color: var(--s8); }

        /* List Styles */
        .list-container { padding: 20px; }
        .tx-card { background: white; padding: 16px; border-radius: 16px; margin-bottom: 12px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 2px 8px rgba(0,0,0,0.02); }
        .tx-title { font-weight: 700; display: block; font-size: 15px; }
        .tx-sub { font-size: 12px; color: #999; }
        .tx-amt { font-weight: 800; font-size: 16px; }
        .tx-amt.revenu { color: var(--g); }
        .tx-amt.depense { color: var(--r); }

        /* Floating Button */
        .fab { position: fixed; bottom: 30px; right: 20px; width: 60px; height: 60px; background: var(--s8); color: white; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 30px; border: none; box-shadow: 0 8px 24px rgba(0,0,0,0.2); cursor: pointer; }

        /* Overlays */
        .ov { display:none; position:fixed; inset:0; background:rgba(0,0,0,0.6); z-index:1000; align-items:flex-end; backdrop-filter: blur(4px); }
        .ov.on { display:flex; }
        .mo { background:white; width:100%; border-radius: 24px 24px 0 0; padding: 30px 20px; animation: slideUp 0.3s ease-out; }
        @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
        
        .field { margin-bottom: 15px; }
        .field label { display: block; font-size: 12px; font-weight: 700; color: #666; margin-bottom: 6px; }
        .field input, .field select { width: 100%; padding: 14px; border: 1.5px solid var(--s2); border-radius: 12px; box-sizing: border-box; font-size: 16px; }

        /* Toast */
        .toast { position: fixed; top: 20px; left: 50%; transform: translateX(-50%) translateY(-100px); background: var(--s8); color: white; padding: 12px 24px; border-radius: 50px; font-weight: 600; transition: 0.5s; z-index: 2000; }
        .toast.on { transform: translateX(-50%) translateY(0); }
    </style>
</head>
<body>

<div id="app" style="display:none">
    <div class="header">
        <span class="solde-label">Solde Actuel</span>
        <div class="solde-val" id="db-solde">0 F</div>
        
        <div class="dash-grid">
            <div class="dash-card green">
                <span class="dash-sub">Revenu Jour</span>
                <span class="dash-num" id="db-rev-day">+0</span>
            </div>
            <div class="dash-card red">
                <span class="dash-sub">Dépense Jour</span>
                <span class="dash-num" id="db-dep-day">-0</span>
            </div>
            <div class="dash-card">
                <span class="dash-sub">Total Mois</span>
                <span class="dash-num" id="db-rev-month">0 F</span>
            </div>
            <div class="dash-card">
                <span class="dash-sub">Solde Mois</span>
                <span class="dash-num" id="db-solde-month">0 F</span>
            </div>
        </div>
    </div>

    <div class="tabs">
        <button class="tab-btn" id="btn-revenu" onclick="switchTab('revenu')">REVENUS</button>
        <button class="tab-btn active" id="btn-depense" onclick="switchTab('depense')">DÉPENSES</button>
    </div>

    <div class="list-container" id="tx-list">
        </div>

    <button class="fab" onclick="showOverlay('ov-add')">+</button>
</div>

<div class="ov" id="ov-add">
    <div class="mo">
        <h3 style="margin:0 0 20px">Ajouter une opération</h3>
        <div class="field">
            <label>Libellé / Description</label>
            <input id="in-desc" placeholder="Ex: Salaire, Loyer...">
        </div>
        <div class="field">
            <label>Montant (FCFA)</label>
            <input id="in-amt" type="number" placeholder="0">
        </div>
        <div class="field">
            <label>Date</label>
            <input id="in-date" type="date">
        </div>
        <div class="field">
            <label>Plan / Catégorie</label>
            <select id="in-plan">
                <option value="conso">🛒 Consommation</option>
                <option value="don">🤲 Don</option>
                <option value="epargne">💳 Épargne</option>
                <option value="invest">📈 Investissement</option>
            </select>
        </div>
        <div style="display:flex; gap:10px; margin-top:10px;">
            <button class="tab-btn" style="background:#eee" onclick="hideOverlay('ov-add')">Annuler</button>
            <button class="tab-btn active" id="btn-save" onclick="saveTx()" style="background:var(--s8); color:white">Confirmer l'ajout</button>
        </div>
    </div>
</div>

<div id="toast" class="toast">Élément ajouté !</div>

<script>
    document.getElementById('in-date').valueAsDate = new Date();
</script>

</body>
</html>
