<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Xiif XamXam Finances + Business</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<script>window._fbReady=new Promise(r=>{window._fbResolve=r;});</script>
<script type="module">
import{initializeApp}from"https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import{getAuth,createUserWithEmailAndPassword,signInWithEmailAndPassword,signOut,onAuthStateChanged,updateProfile}from"https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
import{getFirestore,doc,setDoc,getDoc}from"https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";
const firebaseConfig={apiKey:"AIzaSyAPw7tRUftioT_Jap0iupmeSH-jsJdBp5w",authDomain:"xiif-xamxam.firebaseapp.com",projectId:"xiif-xamxam",storageBucket:"xiif-xamxam.firebasestorage.app",messagingSenderId:"518564223931",appId:"1:518564223931:web:0881991192a0cc0003429e"};
const app=initializeApp(firebaseConfig);
const auth=getAuth(app);const db=getFirestore(app);
window._fb={auth,db,createUserWithEmailAndPassword,signInWithEmailAndPassword,signOut,onAuthStateChanged,updateProfile,doc,setDoc,getDoc};
window._fbResolve();
onAuthStateChanged(auth,user=>{
  if(user){window._currentUser=user;if(window._onUserReady)window._onUserReady(user);}
  else{window._currentUser=null;showScreen('auth');}
});
</script>
<style>
:root{
  --g:#16A34A;--gl:#DCFCE7;--gm:#BBF7D0;
  --r:#DC2626;--rl:#FEE2E2;--rm:#FECACA;
  --b:#2563EB;--bl:#DBEAFE;--bm:#BFDBFE;
  --am:#D97706;--al:#FEF3C7;
  --pu:#7C3AED;--pl:#EDE9FE;
  --cy:#0891B2;--cyl:#CFFAFE;--cym:#A5F3FC;
  --pk:#DB2777;--pkl:#FCE7F3;--pkm:#FBCFE8;
  --s0:#F9FAFB;--s1:#F3F4F6;--s2:#E5E7EB;--s3:#D1D5DB;
  --s4:#9CA3AF;--s5:#6B7280;--s6:#4B5563;--s7:#374151;--s8:#1F2937;--s9:#111827;
  --w:#FFF;--sh:0 1px 3px rgba(0,0,0,.08);--shm:0 4px 16px rgba(0,0,0,.12);
  --r6:6px;--r8:8px;--r12:12px;--r16:16px;--r20:20px;
  --font:'Plus Jakarta Sans',system-ui,sans-serif;
  --biz:#0F172A;--biz2:#1E293B;--biz3:#334155;
}
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:var(--font);background:var(--s0);color:var(--s8);min-height:100vh;-webkit-font-smoothing:antialiased;}
input,select,button,textarea{font-family:var(--font);}
button{cursor:pointer;}
#ld{display:flex;align-items:center;justify-content:center;min-height:100vh;flex-direction:column;gap:14px;background:var(--w);}
.spin{width:30px;height:30px;border:3px solid var(--s2);border-top-color:var(--g);border-radius:50%;animation:sp .7s linear infinite;}
@keyframes sp{to{transform:rotate(360deg);}}
#au{display:none;min-height:100vh;background:var(--w);align-items:center;justify-content:center;padding:20px;}
.ac{width:100%;max-width:380px;}
.abrand{font-size:26px;font-weight:800;color:var(--s9);letter-spacing:-1px;margin-bottom:4px;}
.abrand span{color:var(--g);}
.asub{font-size:14px;color:var(--s5);margin-bottom:28px;}
.atabs{display:flex;border-bottom:2px solid var(--s2);margin-bottom:24px;}
.atab{flex:1;padding:11px;text-align:center;font-size:14px;font-weight:600;color:var(--s5);cursor:pointer;border-bottom:2.5px solid transparent;margin-bottom:-2px;transition:.15s;}
.atab.on{color:var(--g);border-bottom-color:var(--g);}
.aform{display:flex;flex-direction:column;gap:14px;}
.fl label{font-size:12px;font-weight:700;color:var(--s7);display:block;margin-bottom:5px;text-transform:uppercase;letter-spacing:.3px;}
.fl input,.fl select{width:100%;border:1.5px solid var(--s2);border-radius:var(--r8);padding:11px 13px;font-size:15px;outline:none;transition:.15s;background:var(--w);color:var(--s8);}
.fl input:focus,.fl select:focus{border-color:var(--g);box-shadow:0 0 0 3px rgba(22,163,74,.08);}
.aerr{background:var(--rl);border:1px solid var(--rm);color:var(--r);padding:10px 13px;border-radius:var(--r8);font-size:13px;font-weight:500;display:none;}
.aerr.on{display:block;}
.bpri{width:100%;background:var(--g);color:var(--w);border:none;border-radius:var(--r8);padding:13px;font-size:15px;font-weight:700;transition:.15s;}
.bpri:hover{background:#15803D;}.bpri:disabled{opacity:.55;cursor:not-allowed;}
#ap{display:none;flex-direction:column;min-height:100vh;}
#ap.on{display:flex;}
.mode-bar{display:flex;background:var(--w);border-bottom:1px solid var(--s2);position:sticky;top:0;z-index:101;}
.mode-tab{flex:1;padding:14px;font-size:13px;font-weight:700;text-align:center;cursor:pointer;border-bottom:3px solid transparent;transition:.15s;color:var(--s5);display:flex;align-items:center;justify-content:center;gap:6px;}
.mode-tab.on.perso{color:var(--g);border-bottom-color:var(--g);background:var(--gl);}
.mode-tab.on.biz{color:var(--cy);border-bottom-color:var(--cy);background:var(--cyl);}
.mode-tab:hover{background:var(--s1);}
.tnav{background:var(--w);border-bottom:1px solid var(--s2);display:flex;align-items:center;padding:0 16px;position:sticky;top:53px;z-index:100;box-shadow:0 1px 3px rgba(0,0,0,.05);}
.tnav.bm{background:var(--biz);border-color:var(--biz3);}
.tbrand{font-size:15px;font-weight:800;color:var(--s9);letter-spacing:-.3px;margin-right:16px;padding:16px 0;white-space:nowrap;flex-shrink:0;}
.tnav.bm .tbrand{color:var(--w);}
.ttabs{display:flex;overflow-x:auto;flex:1;}.ttabs::-webkit-scrollbar{display:none;}
.ntab{padding:17px 13px;font-size:13px;font-weight:600;color:var(--s5);cursor:pointer;border-bottom:2.5px solid transparent;white-space:nowrap;transition:.15s;flex-shrink:0;}
.ntab:hover{color:var(--s8);}.ntab.on{color:var(--g);border-bottom-color:var(--g);}
.tnav.bm .ntab{color:#94A3B8;}.tnav.bm .ntab:hover{color:var(--w);}
.tnav.bm .ntab.on{color:var(--cy);border-bottom-color:var(--cy);}
.tnav-r{display:flex;align-items:center;gap:8px;margin-left:auto;padding-left:10px;flex-shrink:0;}
.ubtn{display:flex;align-items:center;gap:7px;padding:6px 11px;border-radius:var(--r8);border:1.5px solid var(--s2);background:var(--w);font-size:13px;font-weight:600;color:var(--s7);transition:.15s;}
.ubtn:hover{background:var(--s1);}
.tnav.bm .ubtn{background:var(--biz2);border-color:var(--biz3);color:#CBD5E1;}
.av{width:27px;height:27px;border-radius:50%;background:var(--g);color:var(--w);font-size:11px;font-weight:800;display:flex;align-items:center;justify-content:center;flex-shrink:0;}
.abody{display:flex;flex:1;overflow:hidden;}
.sb{width:220px;background:var(--w);border-right:1px solid var(--s2);flex-shrink:0;display:flex;flex-direction:column;height:calc(100vh - 107px);position:sticky;top:107px;overflow-y:auto;}
.bsb{background:var(--biz);border-color:var(--biz3);}
.sb::-webkit-scrollbar{width:3px;}.sb::-webkit-scrollbar-thumb{background:var(--s2);border-radius:99px;}
.bsb::-webkit-scrollbar-thumb{background:var(--biz3);}
.sb-qadd{padding:12px 10px 8px;}
.sb-ql{font-size:10px;font-weight:700;color:var(--s4);text-transform:uppercase;letter-spacing:.5px;margin-bottom:7px;}
.bsb .sb-ql{color:#64748B;}
.sb-btns{display:grid;grid-template-columns:1fr 1fr;gap:5px;}
.qb{border:none;border-radius:var(--r6);padding:8px 5px;font-size:11px;font-weight:700;transition:.12s;display:flex;align-items:center;justify-content:center;gap:3px;}
.qb.qg{background:var(--gl);color:var(--g);}.qb.qg:hover{background:var(--gm);}
.qb.qr{background:var(--rl);color:var(--r);}.qb.qr:hover{background:var(--rm);}
.qb.qa{background:var(--al);color:var(--am);grid-column:span 2;}.qb.qa:hover{background:#FDE68A;}
.qb.qep{background:var(--bl);color:var(--b);grid-column:span 2;}.qb.qep:hover{background:var(--bm);}
.qb.qcy{background:var(--cyl);color:var(--cy);grid-column:span 2;}.qb.qcy:hover{background:var(--cym);}
.qb.qpk{background:var(--pkl);color:var(--pk);}.qb.qpk:hover{background:var(--pkm);}
.sdiv{height:1px;background:var(--s2);margin:6px 10px;}
.bsb .sdiv{background:var(--biz3);}
.sb-nav{flex:1;padding:6px 8px;}
.sni{display:flex;align-items:center;gap:9px;padding:9px 11px;border-radius:var(--r8);cursor:pointer;font-size:13px;font-weight:600;color:var(--s6);transition:.12s;margin-bottom:1px;}
.sni:hover{background:var(--s1);color:var(--s8);}.sni.on{background:var(--gl);color:var(--g);}
.bsb .sni{color:#94A3B8;}.bsb .sni:hover{background:var(--biz2);color:var(--w);}
.bsb .sni.on{background:rgba(8,145,178,.18);color:var(--cy);}
.sni .ico{font-size:15px;width:18px;text-align:center;flex-shrink:0;}
.sni .lbl{flex:1;}
.badge{font-size:10px;font-weight:700;background:var(--gl);color:var(--g);padding:2px 6px;border-radius:99px;}
.bsb .badge{background:rgba(8,145,178,.2);color:var(--cy);}
.ssec{padding:4px 11px 2px;font-size:10px;font-weight:700;color:var(--s4);text-transform:uppercase;letter-spacing:.5px;}
.bsb .ssec{color:#475569;}
.sb-bot{padding:10px 10px 12px;border-top:1px solid var(--s2);}
.bsb .sb-bot{border-color:var(--biz3);}
.sb-bot label{font-size:10px;font-weight:700;color:var(--s5);text-transform:uppercase;letter-spacing:.3px;margin-bottom:5px;display:flex;align-items:center;gap:4px;}
.bsb .sb-bot label{color:#64748B;}
.sb-bot select{width:100%;border:1.5px solid var(--s2);border-radius:var(--r8);padding:8px 10px;font-size:13px;font-weight:600;color:var(--s7);outline:none;background:var(--w);}
.bsb .sb-bot select{background:var(--biz2);border-color:var(--biz3);color:#CBD5E1;}
.sdo{display:inline-block;width:6px;height:6px;border-radius:50%;background:var(--g);}
.sdo.sy{background:var(--am);animation:pu .8s ease-in-out infinite;}
@keyframes pu{0%,100%{opacity:1;}50%{opacity:.3;}}
.cnt{flex:1;overflow-y:auto;padding:22px;}
.pg{display:none;animation:fi .18s ease;}
.pg.on{display:block;}
@keyframes fi{from{opacity:0;transform:translateY(3px);}to{opacity:1;}}
.card{background:var(--w);border:1px solid var(--s2);border-radius:var(--r12);padding:18px;box-shadow:var(--sh);}
.kg{display:grid;grid-template-columns:repeat(auto-fill,minmax(160px,1fr));gap:11px;margin-bottom:18px;}
.kpi{background:var(--w);border:1px solid var(--s2);border-radius:var(--r12);padding:14px 16px;box-shadow:var(--sh);}
.kl{font-size:11px;font-weight:700;color:var(--s5);text-transform:uppercase;letter-spacing:.4px;margin-bottom:5px;}
.kv{font-size:20px;font-weight:800;letter-spacing:-.4px;}
.ks{font-size:11px;color:var(--s4);margin-top:2px;}
.prow{display:grid;grid-template-columns:repeat(auto-fill,minmax(180px,1fr));gap:10px;margin-bottom:18px;}
.pchip{border-radius:var(--r12);padding:13px 15px;cursor:pointer;transition:.15s;border:1.5px solid transparent;}
.pchip:hover{transform:translateY(-1px);box-shadow:var(--shm);}
.pcn{font-size:11px;font-weight:700;margin-bottom:3px;}
.pcb{font-size:17px;font-weight:800;letter-spacing:-.4px;}
.pcs{font-size:11px;margin-top:3px;opacity:.7;}
.pr{height:5px;border-radius:99px;overflow:hidden;}
.prt{height:100%;border-radius:99px;transition:.3s;}
.btn{display:inline-flex;align-items:center;gap:6px;padding:8px 14px;border-radius:var(--r8);font-size:13px;font-weight:600;border:none;transition:.15s;}
.bg{background:var(--g);color:var(--w);}.bg:hover{background:#15803D;}.bg:disabled{opacity:.5;cursor:not-allowed;}
.bo{background:var(--w);border:1.5px solid var(--s2);color:var(--s7);}.bo:hover{border-color:var(--s4);}
.br2{background:var(--rl);color:var(--r);border:1.5px solid var(--rm);}.br2:hover{background:var(--rm);}
.bb{background:var(--bl);color:var(--b);border:1.5px solid var(--bm);}.bb:hover{background:var(--bm);}
.bcy{background:var(--cyl);color:var(--cy);border:1.5px solid var(--cym);}.bcy:hover{background:var(--cym);}
.bam{background:var(--al);color:var(--am);border:1.5px solid #FDE68A;}
.bpk{background:var(--pkl);color:var(--pk);border:1.5px solid var(--pkm);}.bpk:hover{background:var(--pkm);}
.bsm{padding:6px 11px;font-size:12px;}
.bico{background:none;border:none;padding:5px;border-radius:var(--r6);display:inline-flex;align-items:center;transition:.15s;}
.bico:hover{background:var(--s1);}.bico.ed:hover{color:var(--b);}.bico.dl:hover{color:var(--r);}
.fs label{font-size:12px;font-weight:700;color:var(--s6);margin-bottom:5px;display:block;text-transform:uppercase;letter-spacing:.3px;}
.fs input,.fs select{width:100%;border:1.5px solid var(--s2);border-radius:var(--r8);padding:9px 11px;font-size:13px;color:var(--s8);outline:none;transition:.15s;background:var(--w);}
.fs input:focus,.fs select:focus{border-color:var(--g);}
.txl{display:flex;flex-direction:column;gap:5px;}
.txi{background:var(--w);border:1px solid var(--s2);border-radius:var(--r12);padding:11px 14px;display:flex;align-items:center;gap:10px;transition:.12s;}
.txi:hover{border-color:var(--s3);box-shadow:var(--sh);}
.txd{width:8px;height:8px;border-radius:50%;flex-shrink:0;}
.txd.rv{background:var(--g);}.txd.dp{background:var(--r);}.txd.ep{background:var(--b);}
.txinf{flex:1;min-width:0;}
.txdsc{font-size:14px;font-weight:600;color:var(--s8);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.txmt{font-size:12px;color:var(--s5);margin-top:1px;}
.txam{font-size:15px;font-weight:800;letter-spacing:-.3px;flex-shrink:0;}
.txam.p{color:var(--g);}.txam.n{color:var(--r);}.txam.b{color:var(--b);}
.txac{display:flex;gap:3px;opacity:0;transition:.15s;}
.txi:hover .txac{opacity:1;}
.est{text-align:center;padding:44px 20px;color:var(--s4);}
.eico{font-size:34px;margin-bottom:8px;}
.tbar{display:flex;border-bottom:2px solid var(--s2);margin-bottom:18px;}
.tbtn{padding:10px 15px;font-size:13px;font-weight:600;color:var(--s5);cursor:pointer;border-bottom:2px solid transparent;margin-bottom:-2px;transition:.15s;}
.tbtn.on{color:var(--g);border-bottom-color:var(--g);}
.tbtn.cy.on{color:var(--cy);border-bottom-color:var(--cy);}
.dt{width:100%;border-collapse:collapse;}
.dt th{text-align:left;padding:9px 11px;font-size:11px;font-weight:700;color:var(--s5);text-transform:uppercase;letter-spacing:.4px;border-bottom:2px solid var(--s2);}
.dt td{padding:10px 11px;font-size:13px;border-bottom:1px solid var(--s1);color:var(--s7);}
.dt tr:hover td{background:var(--s0);}
.dt tfoot td{padding:10px 11px;font-weight:700;border-top:2px solid var(--s2);background:var(--s0);}
.ov{display:none;position:fixed;inset:0;background:rgba(0,0,0,.35);z-index:500;align-items:center;justify-content:center;padding:16px;backdrop-filter:blur(2px);}
.ov.on{display:flex;}
.mo{background:var(--w);border-radius:var(--r20);padding:24px;width:100%;max-width:460px;box-shadow:var(--shm);max-height:88vh;overflow-y:auto;}
.mo h2{font-size:18px;font-weight:800;color:var(--s9);margin-bottom:18px;}
.mof{display:flex;flex-direction:column;gap:13px;}
.moa{display:flex;gap:9px;justify-content:flex-end;margin-top:20px;}
.cmo{background:var(--w);border-radius:var(--r20);padding:26px;width:100%;max-width:340px;box-shadow:var(--shm);text-align:center;}
.cmo .ci{font-size:38px;margin-bottom:10px;}
.cmo .ct{font-size:16px;font-weight:800;color:var(--s9);margin-bottom:5px;}
.cmo .cd{font-size:13px;color:var(--s5);margin-bottom:22px;}
.toast{position:fixed;bottom:22px;left:50%;transform:translateX(-50%) translateY(8px);background:var(--s9);color:var(--w);padding:11px 18px;border-radius:var(--r12);font-size:13px;font-weight:600;z-index:9999;opacity:0;transition:.25s;pointer-events:none;display:flex;align-items:center;gap:9px;white-space:nowrap;box-shadow:var(--shm);}
.toast.on{opacity:1;transform:translateX(-50%) translateY(0);}
.toast.sg{background:var(--g);}.toast.er{background:var(--r);}
.undo{background:rgba(255,255,255,.2);border:none;color:var(--w);padding:3px 9px;border-radius:5px;font-size:12px;font-weight:700;cursor:pointer;margin-left:3px;}
.dgrid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:12px;}
.dc{background:var(--w);border:1.5px solid var(--rm);border-radius:var(--r12);padding:15px;}
.dc.ok{border-color:var(--gm);background:var(--gl);}
.dcn{font-size:14px;font-weight:800;color:var(--r);margin-bottom:8px;}
.dc.ok .dcn{color:var(--g);}
.dr{display:flex;justify-content:space-between;font-size:13px;padding:3px 0;}
.dl2{color:var(--s5);}.dv2{font-weight:600;}
.efchips{display:flex;gap:7px;flex-wrap:wrap;margin-bottom:12px;}
.ec{padding:6px 12px;border-radius:99px;font-size:12px;font-weight:700;cursor:pointer;border:1.5px solid var(--s2);background:var(--w);color:var(--s6);transition:.15s;}
.ec.on{background:var(--g);color:var(--w);border-color:var(--g);}
.pp{background:var(--w);border:1px solid var(--s2);border-radius:var(--r12);padding:14px;margin-bottom:10px;}
.pph{display:flex;align-items:center;justify-content:space-between;margin-bottom:10px;}
.ppn{font-size:13px;font-weight:700;}
.ppi{border:1.5px solid var(--s2);border-radius:var(--r8);padding:6px 9px;font-size:15px;font-weight:800;width:68px;text-align:center;outline:none;}
input[type=range]{width:100%;accent-color:var(--g);}
.bge{display:inline-flex;align-items:center;padding:2px 8px;border-radius:99px;font-size:11px;font-weight:700;}
.bgg{background:var(--gl);color:var(--g);}.bgr{background:var(--rl);color:var(--r);}
.bgb{background:var(--bl);color:var(--b);}.bga{background:var(--al);color:var(--am);}
.bgp{background:var(--pl);color:var(--pu);}.bgs{background:var(--s1);color:var(--s6);}
.bgcy{background:var(--cyl);color:var(--cy);}.bgpk{background:var(--pkl);color:var(--pk);}
.flex{display:flex;}.gi2{gap:8px;}.gi3{gap:12px;}.aic{align-items:center;}.jb{justify-content:space-between;}.f1{flex:1;}.fw{flex-wrap:wrap;}
.mt2{margin-top:8px;}.mt3{margin-top:12px;}.mt4{margin-top:16px;}.mb3{margin-bottom:12px;}.mb4{margin-bottom:16px;}
.tsm{font-size:13px;}.txs{font-size:12px;}.tmu{color:var(--s5);}
.fb{font-weight:700;}.tg{color:var(--g);}.tr{color:var(--r);}.tb{color:var(--b);}
.scx{overflow-x:auto;}
.ib{border-radius:var(--r8);padding:12px 14px;display:flex;gap:10px;align-items:flex-start;font-size:13px;}
.ib-g{background:var(--gl);border:1px solid var(--gm);color:var(--g);}
.ib-b{background:var(--bl);border:1px solid var(--bm);color:var(--b);}
.ib-r{background:var(--rl);border:1px solid var(--rm);color:var(--r);}
.ib-a{background:var(--al);border:1px solid #FDE68A;color:var(--am);}
.ib-cy{background:var(--cyl);border:1px solid var(--cym);color:var(--cy);}
.hamb{display:none;background:none;border:none;font-size:20px;padding:6px;border-radius:var(--r6);color:var(--s7);margin-right:6px;}
/* BIZ */
.biz-hdr{background:linear-gradient(135deg,var(--biz),var(--biz2));border-radius:var(--r16);padding:20px 22px;margin-bottom:18px;color:var(--w);position:relative;overflow:hidden;}
.biz-hdr::after{content:'';position:absolute;top:-30px;right:-30px;width:120px;height:120px;border-radius:50%;background:rgba(8,145,178,.15);pointer-events:none;}
.biz-sel-row{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:18px;}
.biz-card{background:var(--w);border:2px solid var(--s2);border-radius:var(--r12);padding:13px 16px;cursor:pointer;transition:.15s;min-width:150px;}
.biz-card:hover{border-color:var(--cy);box-shadow:var(--sh);}
.biz-card.on{border-color:var(--cy);background:var(--cyl);}
.biz-card-nm{font-size:13px;font-weight:800;color:var(--s8);}
.biz-card.on .biz-card-nm{color:var(--cy);}
.biz-card-tp{font-size:11px;color:var(--s5);margin-top:2px;}
.biz-card-add{border:2px dashed var(--s3);background:var(--s0);display:flex;align-items:center;justify-content:center;gap:6px;color:var(--s5);font-size:13px;font-weight:700;}
.biz-card-add:hover{border-color:var(--cy);color:var(--cy);background:var(--cyl);}
.cmd-card{background:var(--w);border:1px solid var(--s2);border-radius:var(--r12);padding:14px;margin-bottom:10px;transition:.15s;}
.cmd-card:hover{box-shadow:var(--shm);border-color:var(--s3);}
.cmd-cl{font-size:14px;font-weight:800;color:var(--s8);}
.cmd-ds{font-size:12px;color:var(--s5);margin-top:2px;}
.cmd-st{padding:3px 9px;border-radius:99px;font-size:11px;font-weight:700;}
.cmd-st.en-cours{background:var(--al);color:var(--am);}
.cmd-st.termine{background:var(--gl);color:var(--g);}
.cmd-st.annule{background:var(--s1);color:var(--s5);}
.cmd-fin{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-top:10px;}
.cfi{background:var(--s0);border-radius:var(--r8);padding:8px 10px;text-align:center;}
.cfl{font-size:10px;font-weight:700;color:var(--s5);text-transform:uppercase;letter-spacing:.3px;}
.cfv{font-size:14px;font-weight:800;margin-top:2px;}
.chg-g{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:10px;margin-bottom:14px;}
.chg-i{background:var(--w);border:1.5px solid var(--s2);border-radius:var(--r12);padding:13px;}
.sal-bn{background:linear-gradient(135deg,#0F172A,#1E3A5F);border-radius:var(--r16);padding:22px;margin-bottom:18px;color:var(--w);display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:14px;}
.sal-bn .sl{font-size:12px;font-weight:700;color:#94A3B8;text-transform:uppercase;letter-spacing:.5px;margin-bottom:4px;}
.sal-bn .sa{font-size:28px;font-weight:800;letter-spacing:-.5px;}
.sal-bn .ss{font-size:12px;color:#64748B;margin-top:4px;}
.sal-bt{background:var(--cy);color:var(--w);border:none;border-radius:var(--r12);padding:13px 22px;font-size:14px;font-weight:800;cursor:pointer;transition:.15s;display:flex;align-items:center;gap:8px;}
.sal-bt:hover{background:#0E7490;}
.fou-g{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:10px;}
.fou-c{background:var(--w);border:1.5px solid var(--s2);border-radius:var(--r12);padding:13px;}
.vi{background:var(--w);border:1px solid var(--s2);border-radius:var(--r12);padding:11px 14px;display:flex;align-items:center;gap:10px;margin-bottom:6px;}
.vico{width:36px;height:36px;border-radius:50%;background:var(--cyl);display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0;}
@media(max-width:768px){
  .sb{display:none;position:fixed;top:0;left:0;height:100vh;z-index:200;box-shadow:var(--shm);}
  .sb.mob{display:flex;}.ttabs{display:flex;}.hamb{display:block;}.cnt{padding:14px;}
  .kg{grid-template-columns:1fr 1fr;}.prow{grid-template-columns:1fr 1fr;}
  .cmd-fin{grid-template-columns:1fr 1fr;}
  .sal-bn{flex-direction:column;}.sal-bt{width:100%;justify-content:center;}
}
@media(min-width:769px){.ttabs{display:none;}}

/* V3 - Graphiques, Alertes, Objectifs, PWA, IA */
.chart-wrap{position:relative;height:220px;margin:12px 0;}
.alert-bar{background:var(--rl);border:1px solid var(--rm);border-radius:var(--r8);padding:10px 14px;display:flex;align-items:center;gap:10px;margin-bottom:8px;}
.alert-bar .ab-ico{font-size:16px;flex-shrink:0;}
.alert-bar .ab-txt{font-size:13px;color:var(--r);font-weight:600;flex:1;}
.alert-bar.warn{background:var(--al);border-color:#FDE68A;}
.alert-bar.warn .ab-txt{color:var(--am);}
.obj-card{background:var(--w);border:1.5px solid var(--s2);border-radius:var(--r12);padding:14px;margin-bottom:10px;}
.obj-name{font-size:14px;font-weight:700;color:var(--s8);}
.obj-prog-wrap{margin:10px 0 4px;}
.obj-prog-bg{height:10px;background:var(--s1);border-radius:99px;overflow:hidden;}
.obj-prog-fill{height:100%;border-radius:99px;transition:.4s;}
.rec-item{background:var(--w);border:1px solid var(--s2);border-radius:var(--r12);padding:11px 14px;margin-bottom:6px;display:flex;align-items:center;gap:10px;}
.rec-ico{width:34px;height:34px;border-radius:50%;background:var(--bl);display:flex;align-items:center;justify-content:center;font-size:14px;flex-shrink:0;}
.ia-box{background:linear-gradient(135deg,#0F172A,#1E293B);border-radius:var(--r16);padding:20px;color:var(--w);margin-bottom:18px;}
.ia-resp{background:var(--s0);border:1px solid var(--s2);border-radius:var(--r12);padding:16px;font-size:13px;line-height:1.7;color:var(--s7);margin-top:12px;white-space:pre-wrap;}
.fact-pill{display:inline-flex;align-items:center;gap:5px;background:var(--cyl);color:var(--cy);border-radius:99px;padding:4px 12px;font-size:12px;font-weight:700;margin:3px;}
.inv-card{background:var(--w);border:1.5px solid var(--s2);border-radius:var(--r12);padding:14px;margin-bottom:10px;}
.inv-header{display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:8px;}
.inv-num{font-size:12px;font-weight:700;color:var(--s4);}
.inv-client{font-size:15px;font-weight:800;color:var(--s8);}
.inv-status{padding:3px 10px;border-radius:99px;font-size:11px;font-weight:700;}
.inv-status.paye{background:var(--gl);color:var(--g);}
.inv-status.attente{background:var(--al);color:var(--am);}
.inv-status.envoye{background:var(--bl);color:var(--b);}
.pdf-btn{background:var(--rl);color:var(--r);border:1.5px solid var(--rm);border-radius:var(--r8);padding:6px 12px;font-size:12px;font-weight:700;cursor:pointer;transition:.15s;}
.pdf-btn:hover{background:var(--rm);}
</style>
</head>
<body>
<div id="ld"><div class="spin"></div><p style="font-size:13px;color:var(--s4);">Chargement…</p></div>
<!-- AUTH -->
<div id="au">
  <div class="ac">
    <div class="abrand">Xiif <span>XamXam</span></div>
    <div class="asub">Finances Personnelles &amp; Business</div>
    <div class="atabs">
      <div class="atab on" id="tl" onclick="stab('l')">Se connecter</div>
      <div class="atab" id="tr2" onclick="stab('r')">Créer un compte</div>
    </div>
    <div class="aerr" id="aerr"></div>
    <div id="fl" class="aform">
      <div class="fl"><label>Email</label><input id="le" type="email" placeholder="ton@email.com"/></div>
      <div class="fl"><label>Mot de passe</label><input id="lp" type="password" placeholder="••••••••" onkeydown="if(event.key==='Enter')doLogin()"/></div>
      <button class="bpri" onclick="doLogin()">Se connecter</button>
    </div>
    <div id="fr" class="aform" style="display:none;">
      <div class="fl"><label>Prénom &amp; Nom</label><input id="rn" placeholder="Pape Coly SARR"/></div>
      <div class="fl"><label>Email</label><input id="re" type="email" placeholder="ton@email.com"/></div>
      <div class="fl"><label>Mot de passe (min. 6 car.)</label><input id="rp2" type="password" placeholder="••••••••" onkeydown="if(event.key==='Enter')doRegister()"/></div>
      <button class="bpri" onclick="doRegister()">Créer mon compte</button>
    </div>
  </div>
</div>

<!-- APP -->
<div id="ap">
  <!-- MODE BAR -->
  <div class="mode-bar">
    <div class="mode-tab on perso" id="mode-perso" onclick="setMode('perso')">👤 Finances Personnelles</div>
    <div class="mode-tab biz" id="mode-biz" onclick="setMode('biz')">🏢 Gérer mon Business</div>
  </div>
  <nav class="tnav" id="mnav">
    <button class="hamb" onclick="togSB()">☰</button>
    <div class="tbrand" id="nbrand">Xiif XamXam</div>
    <div class="ttabs">
      <div class="ntab on" onclick="gp('db')" data-p="db">🏠</div>
      <div class="ntab" onclick="gp('tx')" data-p="tx">💳</div>
      <div class="ntab" onclick="gp('plans')" data-p="plans">📊</div>
      <div class="ntab" onclick="gp('dettes')" data-p="dettes">⚠️</div>
      <div class="ntab" onclick="gp('export')" data-p="export">⬇️</div>
      <div class="ntab" onclick="gp('param')" data-p="param">⚙️</div>
    </div>
    <div class="tnav-r">
      <button class="ubtn" onclick="doLogout()">
        <div class="av" id="uav">?</div><span id="unm">…</span><span style="color:var(--s4);font-size:12px;">⏻</span>
      </button>
    </div>
  </nav>
  <div class="abody">
    <!-- SIDEBAR PERSO -->
    <aside class="sb" id="sb">
      <div class="sb-qadd">
        <div class="sb-ql">Ajouter</div>
        <div class="sb-btns">
          <button class="qb qg" onclick="oaTx('revenu')">+ Revenu</button>
          <button class="qb qr" onclick="oaTx('depense')">+ Dépense</button>
          <button class="qb qa" onclick="oaND()">⚠️ Contracter dette</button>
          <button class="qb qep" onclick="oaRet()">↩ Retrait épargne</button>
        </div>
      </div>
      <div class="sdiv"></div>
      <nav class="sb-nav">
        <div class="sni on" onclick="gp('db')" data-p="db"><span class="ico">🏠</span><span class="lbl">Tableau de bord</span></div>
        <div class="sni" onclick="gp('tx')" data-p="tx"><span class="ico">💳</span><span class="lbl">Transactions</span></div>
        <div class="sdiv"></div>
        <div class="ssec">Plans</div>
        <div class="sni" onclick="gp('plans')" data-p="plans"><span class="ico">📊</span><span class="lbl">Tous les plans</span></div>
        <div class="sni" onclick="gp('don')" data-p="don"><span class="ico">🤲</span><span class="lbl">Plan Don</span><span class="badge" id="sb-don">10%</span></div>
        <div class="sni" onclick="gp('epargne')" data-p="epargne"><span class="ico">💳</span><span class="lbl">Plan Épargne</span><span class="badge" id="sb-ep">10%</span></div>
        <div class="sni" onclick="gp('invest')" data-p="invest"><span class="ico">📈</span><span class="lbl">Investissement</span><span class="badge" id="sb-inv">5%</span></div>
        <div class="sni" onclick="gp('dettes')" data-p="dettes"><span class="ico">⚠️</span><span class="lbl">Dettes</span><span class="badge" id="sb-det">10%</span></div>
        <div class="sni" onclick="gp('conso')" data-p="conso"><span class="ico">🛒</span><span class="lbl">Consommation</span><span class="badge" id="sb-con">65%</span></div>
        <div class="sdiv"></div>
        <div class="sni" onclick="gp('charts')" data-p="charts"><span class="ico">📊</span><span class="lbl">Graphiques</span></div>
        <div class="sni" onclick="gp('objectifs')" data-p="objectifs"><span class="ico">🎯</span><span class="lbl">Objectifs épargne</span></div>
        <div class="sni" onclick="gp('recurrentes')" data-p="recurrentes"><span class="ico">🔄</span><span class="lbl">Récurrentes</span></div>
        <div class="sni" onclick="gp('ia')" data-p="ia"><span class="ico">🤖</span><span class="lbl">Analyse IA</span></div>
        <div class="sdiv"></div>
        <div class="sni" onclick="gp('export')" data-p="export"><span class="ico">⬇️</span><span class="lbl">Export Excel</span></div>
        <div class="sni" onclick="gp('param')" data-p="param"><span class="ico">⚙️</span><span class="lbl">Paramètres</span></div>
      </nav>
      <div class="sb-bot">
        <label><span class="sdo" id="sdo"></span> Mois actif</label>
        <select id="msel" onchange="chMois(this.value)"></select>
      </div>
    </aside>
    <!-- SIDEBAR BIZ -->
    <aside class="sb bsb" id="sbb" style="display:none;">
      <div class="sb-qadd">
        <div class="sb-ql">Actions rapides</div>
        <div class="sb-btns">
          <button class="qb qcy" onclick="oNewCmd()" style="grid-column:span 2">+ Nouvelle commande</button>
          <button class="qb qg" onclick="oNewCharge()">+ Charge fixe</button>
          <button class="qb qpk" onclick="oNewFou()">+ Fournisseur</button>
        </div>
      </div>
      <div class="sdiv"></div>
      <nav class="sb-nav">
        <div class="sni on" onclick="gpB('biz-db')" data-bp="biz-db"><span class="ico">🏢</span><span class="lbl">Dashboard</span></div>
        <div class="sni" onclick="gpB('biz-cmd')" data-bp="biz-cmd"><span class="ico">📋</span><span class="lbl">Commandes clients</span></div>
        <div class="sni" onclick="gpB('biz-charges')" data-bp="biz-charges"><span class="ico">💸</span><span class="lbl">Charges fixes</span></div>
        <div class="sni" onclick="gpB('biz-fou')" data-bp="biz-fou"><span class="ico">🏭</span><span class="lbl">Fournisseurs &amp; Stock</span></div>
        <div class="sni" onclick="gpB('biz-sal')" data-bp="biz-sal"><span class="ico">💰</span><span class="lbl">Mon salaire</span></div>
        <div class="sni" onclick="gpB('biz-factures')" data-bp="biz-factures"><span class="ico">🧾</span><span class="lbl">Factures &amp; Devis</span></div>
        <div class="sni" onclick="gpB('biz-set')" data-bp="biz-set"><span class="ico">⚙️</span><span class="lbl">Mes business</span></div>
      </nav>
      <div class="sb-bot">
        <label>🏢 Business actif</label>
        <select id="bsel" onchange="chBiz(this.value)"></select>
        <div style="height:8px;"></div>
        <label><span class="sdo" id="sdo-b"></span> Mois</label>
        <select id="mselb" onchange="chMoisB(this.value)"></select>
      </div>
    </aside>
    <!-- MAIN CONTENT -->
    <div class="cnt">
      <!-- PERSO PAGES -->
      <div class="pg on" id="pg-db">
        <div class="flex aic jb mb4" style="flex-wrap:wrap;gap:10px;">
          <div><div style="font-size:21px;font-weight:800;letter-spacing:-.5px;" id="dbtit">Tableau de bord</div>
            <div class="tsm tmu mt2"><span class="sdo" id="sdo2"></span> Synchronisé</div></div>
          <div class="flex gi2 fw">
            <button class="btn bg" onclick="oaTx('revenu')">💚 Ajouter un revenu</button>
            <button class="btn br2" onclick="oaTx('depense')">🔴 Ajouter une dépense</button>
            <button class="btn bam" onclick="oaND()">⚠️ Contracter une dette</button>
          </div>
        </div>
        <div class="kg" id="dbkpi"></div>
        <div class="prow" id="dbplans"></div>
        <div class="card"><div class="flex aic jb mb3"><div style="font-size:14px;font-weight:700;">Transactions récentes</div><button class="btn bo bsm" onclick="gp('tx')">Voir tout →</button></div><div id="dbrec"></div></div>
      </div>
      <div class="pg" id="pg-tx">
        <div class="flex aic jb mb4" style="flex-wrap:wrap;gap:10px;">
          <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;">Transactions</div>
          <div class="flex gi2"><button class="btn bg" onclick="oaTx('revenu')">+ Revenu</button><button class="btn br2" onclick="oaTx('depense')">+ Dépense</button></div>
        </div>
        <div class="tbar">
          <div class="tbtn on" onclick="stt('tout')" id="tt-tout">Tout</div>
          <div class="tbtn" onclick="stt('rev')" id="tt-rev">Revenus</div>
          <div class="tbtn" onclick="stt('dep')" id="tt-dep">Dépenses</div>
        </div>
        <div class="flex gi2 fw mb4">
          <div class="fs f1" style="min-width:140px;"><label>Rechercher</label><input id="tsx" placeholder="Description…" oninput="rTx()"/></div>
          <div class="fs" style="width:150px;"><label>Plan</label>
            <select id="tspf" onchange="rTx()">
              <option value="">Tous les plans</option>
              <option value="don">Don</option><option value="epargne">Épargne</option>
              <option value="invest">Investissement</option><option value="dette">Dettes</option>
              <option value="conso">Consommation</option>
            </select>
          </div>
        </div>
        <div id="txlist"></div>
      </div>
      <div class="pg" id="pg-plans"><div style="font-size:21px;font-weight:800;letter-spacing:-.5px;margin-bottom:16px;">Plans budgétaires</div><div id="plc"></div></div>
      <div class="pg" id="pg-don"><div class="flex aic jb mb4"><div style="font-size:21px;font-weight:800;">🤲 Plan Don</div><button class="btn bg bsm" onclick="oaTx('depense');sv('txpl','don');upSub()">+ Dépense Don</button></div><div id="plc-don"></div></div>
      <div class="pg" id="pg-epargne"><div class="flex aic jb mb4" style="flex-wrap:wrap;gap:8px;"><div style="font-size:21px;font-weight:800;">💳 Plan Épargne</div><div class="flex gi2"><button class="btn bg bsm" onclick="oaTx('depense');sv('txpl','epargne');upSub()">+ Épargner</button><button class="btn bb bsm" onclick="oaRet()">↩ Retirer</button></div></div><div id="plc-epargne"></div></div>
      <div class="pg" id="pg-invest"><div class="flex aic jb mb4"><div style="font-size:21px;font-weight:800;">📈 Plan Investissement</div><button class="btn bg bsm" onclick="oaTx('depense');sv('txpl','invest');upSub()">+ Dépense Invest.</button></div><div id="plc-invest"></div></div>
      <div class="pg" id="pg-conso"><div class="flex aic jb mb4"><div style="font-size:21px;font-weight:800;">🛒 Plan Consommation</div><button class="btn bg bsm" onclick="oaTx('depense');sv('txpl','conso');upSub()">+ Dépense</button></div><div id="plc-conso"></div></div>
      <div class="pg" id="pg-dettes">
        <div class="flex aic jb mb4" style="flex-wrap:wrap;gap:10px;">
          <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;">⚠️ Dettes</div>
          <button class="btn bam" onclick="oaND()">⚠️ Contracter une dette</button>
        </div>
        <div id="detc"></div>
      </div>
      <div class="pg" id="pg-export">
        <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;margin-bottom:16px;">Export Excel</div>
        <div class="card mb4">
          <div style="font-size:13px;font-weight:700;margin-bottom:10px;">Période</div>
          <div class="efchips" id="efp"><div class="ec on" onclick="sePer(this,'mois')">Ce mois</div><div class="ec" onclick="sePer(this,'3mois')">3 mois</div><div class="ec" onclick="sePer(this,'6mois')">6 mois</div><div class="ec" onclick="sePer(this,'all')">Tout</div></div>
          <div style="font-size:13px;font-weight:700;margin-bottom:10px;margin-top:14px;">Plan</div>
          <div class="efchips" id="efpl"><div class="ec on" onclick="sePlan(this,'all')">Tous</div><div class="ec" onclick="sePlan(this,'don')">Don</div><div class="ec" onclick="sePlan(this,'epargne')">Épargne</div><div class="ec" onclick="sePlan(this,'invest')">Investissement</div><div class="ec" onclick="sePlan(this,'dette')">Dettes</div><div class="ec" onclick="sePlan(this,'conso')">Consommation</div></div>
          <div id="exprev" class="mt4"></div>
          <button class="btn bg mt4" onclick="doExp()">⬇️ Télécharger Excel</button>
        </div>
      </div>
      <div class="pg" id="pg-param">
        <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;margin-bottom:5px;">Paramètres</div>
        <p class="tsm tmu mb4">Le total de tous les plans doit être égal à 100%.</p>
        <div id="parc"></div>
      </div>
      <!-- BIZ PAGES -->
      <div class="pg" id="pg-biz-db"><div id="bdb"></div></div>
      <div class="pg" id="pg-biz-cmd">
        <div class="flex aic jb mb4" style="flex-wrap:wrap;gap:10px;">
          <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;">📋 Commandes Clients</div>
          <button class="btn bcy" onclick="oNewCmd()">+ Nouvelle commande</button>
        </div>
        <div class="tbar">
          <div class="tbtn cy on" onclick="sttC('tout')" id="ct-tout">Toutes</div>
          <div class="tbtn cy" onclick="sttC('en-cours')" id="ct-en-cours">En cours</div>
          <div class="tbtn cy" onclick="sttC('termine')" id="ct-termine">Terminées</div>
          <div class="tbtn cy" onclick="sttC('annule')" id="ct-annule">Annulées</div>
        </div>
        <div id="cmdlist"></div>
      </div>
      <div class="pg" id="pg-biz-charges">
        <div class="flex aic jb mb4" style="flex-wrap:wrap;gap:10px;">
          <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;">💸 Charges Fixes</div>
          <button class="btn bcy" onclick="oNewCharge()">+ Nouvelle charge</button>
        </div>
        <div id="chglist"></div>
      </div>
      <div class="pg" id="pg-biz-fou">
        <div class="flex aic jb mb4" style="flex-wrap:wrap;gap:10px;">
          <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;">🏭 Fournisseurs &amp; Stock</div>
          <button class="btn bpk" onclick="oNewFou()">+ Fournisseur</button>
        </div>
        <div id="foulist"></div>
      </div>
      <div class="pg" id="pg-biz-sal">
        <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;margin-bottom:16px;">💰 Mon Salaire Dirigeant</div>
        <div id="salc"></div>
      </div>
      <div class="pg" id="pg-biz-set">
        <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;margin-bottom:16px;">⚙️ Mes Business</div>
        <div id="bizsett"></div>
      </div>
      <!-- PAGE GRAPHIQUES -->
      <div class="pg" id="pg-charts">
        <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;margin-bottom:16px;">📊 Graphiques &amp; Évolution</div>
        <div id="charts-content"></div>
      </div>
      <!-- PAGE OBJECTIFS ÉPARGNE -->
      <div class="pg" id="pg-objectifs">
        <div class="flex aic jb mb4" style="flex-wrap:wrap;gap:10px;">
          <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;">🎯 Objectifs d'Épargne</div>
          <button class="btn bg" onclick="oNewObj()">+ Nouvel objectif</button>
        </div>
        <div id="obj-content"></div>
      </div>
      <!-- PAGE TRANSACTIONS RÉCURRENTES -->
      <div class="pg" id="pg-recurrentes">
        <div class="flex aic jb mb4" style="flex-wrap:wrap;gap:10px;">
          <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;">🔄 Transactions Récurrentes</div>
          <button class="btn bg" onclick="oNewRec()">+ Nouvelle récurrence</button>
        </div>
        <div id="rec-content"></div>
      </div>
      <!-- PAGE ANALYSE IA -->
      <div class="pg" id="pg-ia">
        <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;margin-bottom:16px;">🤖 Analyse IA</div>
        <div id="ia-content"></div>
      </div>
      <!-- PAGE FACTURES BIZ -->
      <div class="pg" id="pg-biz-factures">
        <div class="flex aic jb mb4" style="flex-wrap:wrap;gap:10px;">
          <div style="font-size:21px;font-weight:800;letter-spacing:-.5px;">🧾 Factures &amp; Devis</div>
          <button class="btn bcy" onclick="oNewFacture()">+ Nouvelle facture</button>
        </div>
        <div id="factures-list"></div>
      </div>
    </div>
  </div>
</div>

<!-- MODAL TX PERSO -->
<div class="ov" id="ov-tx">
  <div class="mo">
    <h2 id="txmot">Nouvelle transaction</h2>
    <div class="mof">
      <div class="fl"><label>Type</label><select id="txtp" onchange="upPlan()"><option value="revenu">💚 Revenu</option><option value="depense">🔴 Dépense</option></select></div>
      <div class="fl"><label>Description</label><input id="txds" placeholder="Ex: Salaire, Transport…"/></div>
      <div class="fl"><label>Montant (FCFA)</label><input id="txam" type="number" min="1" placeholder="25000"/></div>
      <div class="fl"><label>Date</label><input id="txdt" type="date"/></div>
      <div class="fl" id="txpf"><label>Plan</label>
        <select id="txpl" onchange="upSub()">
          <option value="don">🤲 Plan Don</option><option value="epargne">💳 Plan Épargne</option>
          <option value="invest">📈 Plan Investissement</option><option value="dette">⚠️ Paiement Dettes</option>
          <option value="conso" selected>🛒 Plan Consommation</option>
        </select>
      </div>
      <div class="fl"><label>Sous-catégorie</label><select id="txsc"><option value="">— Général —</option></select></div>
      <div class="fl"><label>Note</label><input id="txnt" placeholder="Note optionnelle…"/></div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-tx')">Annuler</button><button class="btn bg" id="bstx" onclick="savTx()">Enregistrer</button></div>
  </div>
</div>

<!-- MODAL NOUVELLE DETTE -->
<div class="ov" id="ov-nd">
  <div class="mo">
    <h2>⚠️ Contracter une dette</h2>
    <div class="mof">
      <div class="fl"><label>Type</label><select id="nd-tp" onchange="updNDT()"><option value="argent">💰 En argent</option><option value="nature">📦 En nature</option></select></div>
      <div class="fl"><label>Créancier</label><input id="nd-cr" placeholder="Ex: Amadou Ba, BOA…"/></div>
      <div class="fl" id="nd-mf"><label>Montant reçu (FCFA)</label><input id="nd-mt" type="number" placeholder="100000" min="1"/></div>
      <div class="fl" id="nd-nf" style="display:none;"><label>Description (objet/service)</label><input id="nd-ds" placeholder="Ex: Sac de riz, chaussures…"/></div>
      <div class="fl" id="nd-vf" style="display:none;"><label>Valeur estimée (FCFA)</label><input id="nd-vl" type="number" placeholder="25000"/></div>
      <div class="fl"><label>Date</label><input id="nd-dt" type="date"/></div>
      <div class="fl"><label>Date limite remboursement</label><input id="nd-dl" type="date"/></div>
      <div class="fl"><label>Mensualité prévue (FCFA)</label><input id="nd-mn" type="number" value="0"/></div>
      <div class="fl"><label>Note</label><input id="nd-nt" placeholder="Conditions, accord…"/></div>
      <div class="ib ib-g" id="nd-ro">
        <input type="checkbox" id="nd-ar" checked style="margin-top:2px;accent-color:var(--g);width:16px;height:16px;flex-shrink:0;"/>
        <label for="nd-ar" style="font-size:13px;font-weight:600;cursor:pointer;line-height:1.4;">Ajouter aux revenus du mois<br><span style="font-weight:400;color:var(--s6);">Le montant sera réparti dans les 5 plans</span></label>
      </div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-nd')">Annuler</button><button class="btn bg" onclick="savND()">Enregistrer</button></div>
  </div>
</div>

<!-- MODAL MODIFIER DETTE -->
<div class="ov" id="ov-det">
  <div class="mo">
    <h2>Modifier le créancier</h2>
    <div class="mof">
      <div class="fl"><label>Nom du créancier</label><input id="de-nm" placeholder="Ex: Amadou Ba…"/></div>
      <div class="fl"><label>Type</label><select id="de-tp"><option value="argent">💰 Argent</option><option value="nature">📦 Nature</option></select></div>
      <div class="fl"><label>Description</label><input id="de-ds" placeholder="Objet / service…"/></div>
      <div class="fl"><label>Montant total (FCFA)</label><input id="de-tt" type="number" placeholder="150000"/></div>
      <div class="fl"><label>Déjà remboursé (FCFA)</label><input id="de-py" type="number" value="0"/></div>
      <div class="fl"><label>Date limite</label><input id="de-dl" type="date"/></div>
      <div class="fl"><label>Mensualité (FCFA)</label><input id="de-mn" type="number" value="0"/></div>
      <div class="fl"><label>Note</label><input id="de-nt" placeholder="Conditions…"/></div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-det')">Annuler</button><button class="btn bg" onclick="savDet()">Enregistrer</button></div>
  </div>
</div>

<!-- MODAL PAIEMENT DETTE -->
<div class="ov" id="ov-paie">
  <div class="mo">
    <h2>💸 Enregistrer un paiement</h2>
    <div class="mof">
      <div id="pinfo" class="ib ib-r"></div>
      <div class="fl"><label>Montant payé (FCFA)</label><input id="pamt" type="number" placeholder="25000"/></div>
      <div class="fl"><label>Date</label><input id="pdat" type="date"/></div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-paie')">Annuler</button><button class="btn bg" onclick="savPaie()">Confirmer</button></div>
  </div>
</div>

<!-- MODAL RETRAIT ÉPARGNE -->
<div class="ov" id="ov-ret">
  <div class="mo">
    <h2>💳 Retrait d'épargne</h2>
    <div class="mof">
      <div id="ret-info" class="ib ib-g"></div>
      <div class="fl"><label>Montant à retirer (FCFA)</label><input id="ret-am" type="number" placeholder="50000" min="1"/></div>
      <div class="fl"><label>Date</label><input id="ret-dt" type="date"/></div>
      <div class="fl"><label>Motif / Destination</label><input id="ret-mo" placeholder="Ex: Achat téléphone…"/></div>
      <div class="ib ib-b"><span>ℹ️</span><span style="font-size:13px;">Le montant sera <strong>ajouté à tes revenus disponibles</strong>.</span></div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-ret')">Annuler</button><button class="btn bg" onclick="savRet()">Confirmer le retrait</button></div>
  </div>
</div>

<!-- MODAL NOUVEAU BUSINESS -->
<div class="ov" id="ov-nb">
  <div class="mo">
    <h2>🏢 Créer un nouveau business</h2>
    <div class="mof">
      <div class="fl"><label>Nom du business</label><input id="nb-nm" placeholder="Ex: Imprimerie Xiif, Xiif Coaching…"/></div>
      <div class="fl"><label>Secteur d'activité</label>
        <select id="nb-tp">
          <option value="impression">🖨️ Imprimerie / Design</option>
          <option value="commerce">🛒 Commerce / Vente</option>
          <option value="service">🤝 Services / Conseil</option>
          <option value="coaching">🎯 Coaching / Formation</option>
          <option value="resto">🍽️ Restauration / Alimentation</option>
          <option value="tech">💻 Tech / Numérique</option>
          <option value="agri">🌱 Agriculture / Élevage</option>
          <option value="autre">📦 Autre</option>
        </select>
      </div>
      <div class="fl"><label>Description courte</label><input id="nb-ds" placeholder="Ex: Impression supports visuels et textiles…"/></div>
      <div class="fl"><label>Salaire dirigeant prévu / mois (FCFA)</label><input id="nb-sl" type="number" placeholder="400000" min="0"/></div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-nb')">Annuler</button><button class="btn bcy" onclick="savNB()">Créer le business</button></div>
  </div>
</div>

<!-- MODAL NOUVELLE COMMANDE -->
<div class="ov" id="ov-cmd">
  <div class="mo" style="max-width:520px;">
    <h2 id="cmd-mot">📋 Nouvelle commande</h2>
    <div class="mof">
      <div class="fl"><label>Nom du client</label><input id="cmd-cl" placeholder="Ex: Moussa Diop, ONG ENDA…"/></div>
      <div class="fl"><label>Description de la commande</label><input id="cmd-ds" placeholder="Ex: Impression 200 tee-shirts avec logo…"/></div>
      <div class="fl"><label>Date de commande</label><input id="cmd-dt" type="date"/></div>
      <div class="fl"><label>Date de livraison prévue</label><input id="cmd-dl" type="date"/></div>
      <div class="fl"><label>Prix de vente total (FCFA)</label><input id="cmd-px" type="number" placeholder="150000" min="0" oninput="updCmdCalc()"/></div>
      <div style="font-size:13px;font-weight:700;color:var(--s7);">Coûts variables (matières premières)</div>
      <div id="cv-lines" style="display:flex;flex-direction:column;gap:8px;"></div>
      <button class="btn bo bsm" onclick="addCvLine()" style="align-self:flex-start;">+ Ajouter coût variable</button>
      <div class="fl"><label>Statut</label>
        <select id="cmd-st">
          <option value="en-cours">⏳ En cours</option>
          <option value="termine">✅ Terminée</option>
          <option value="annule">❌ Annulée</option>
        </select>
      </div>
      <div class="fl"><label>Note</label><input id="cmd-nt" placeholder="Note optionnelle…"/></div>
      <div class="ib ib-cy" id="cmd-calc" style="display:none;"><div style="font-size:13px;line-height:1.8;" id="cmd-calc-txt"></div></div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-cmd')">Annuler</button><button class="btn bcy" id="bscmd" onclick="savCmd()">Enregistrer la commande</button></div>
  </div>
</div>

<!-- MODAL NOUVELLE CHARGE -->
<div class="ov" id="ov-chg">
  <div class="mo">
    <h2 id="chg-mot">💸 Nouvelle charge fixe</h2>
    <div class="mof">
      <div class="fl"><label>Désignation</label><input id="chg-nm" placeholder="Ex: Électricité, Loyer local…"/></div>
      <div class="fl"><label>Montant (FCFA)</label><input id="chg-am" type="number" placeholder="50000" min="0"/></div>
      <div class="fl"><label>Catégorie</label>
        <select id="chg-ct">
          <option value="loyer">🏠 Loyer / Local</option>
          <option value="energie">⚡ Électricité / Eau</option>
          <option value="internet">📡 Internet / Téléphone</option>
          <option value="salaires">👥 Salaires employés</option>
          <option value="stock">📦 Stock fixe</option>
          <option value="transport">🚗 Transport / Carburant</option>
          <option value="assurance">🛡️ Assurance</option>
          <option value="autre">📋 Autre</option>
        </select>
      </div>
      <div class="fl"><label>Fréquence</label>
        <select id="chg-fr">
          <option value="mensuel">📅 Mensuel</option>
          <option value="trimestriel">📅 Trimestriel</option>
          <option value="annuel">📅 Annuel</option>
          <option value="unique">1️⃣ Unique</option>
        </select>
      </div>
      <div class="fl"><label>Note</label><input id="chg-nt" placeholder="Note optionnelle…"/></div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-chg')">Annuler</button><button class="btn bcy" id="bschg" onclick="savChg()">Enregistrer</button></div>
  </div>
</div>

<!-- MODAL NOUVEAU FOURNISSEUR -->
<div class="ov" id="ov-fou">
  <div class="mo">
    <h2 id="fou-mot">🏭 Nouveau fournisseur</h2>
    <div class="mof">
      <div class="fl"><label>Nom du fournisseur</label><input id="fou-nm" placeholder="Ex: Imprimerie Dakar, Marché Sandaga…"/></div>
      <div class="fl"><label>Produit fourni</label><input id="fou-ct" placeholder="Ex: Tee-shirts, Encres, Papier…"/></div>
      <div class="fl"><label>Contact / Téléphone</label><input id="fou-tl" placeholder="Ex: 77 000 00 00"/></div>
      <div class="fl"><label>Dernier achat (FCFA)</label><input id="fou-ac" type="number" placeholder="0" min="0"/></div>
      <div class="fl"><label>Date dernier achat</label><input id="fou-dt" type="date"/></div>
      <div class="fl"><label>Note</label><input id="fou-nt" placeholder="Conditions, délais livraison…"/></div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-fou')">Annuler</button><button class="btn bpk" id="bsfou" onclick="savFou()">Enregistrer</button></div>
  </div>
</div>

<!-- MODAL SE VERSER SALAIRE -->
<div class="ov" id="ov-sal">
  <div class="mo">
    <h2>💰 Se verser son salaire</h2>
    <div class="mof">
      <div id="sal-inf" class="ib ib-cy" style="flex-direction:column;gap:4px;"></div>
      <div class="fl"><label>Montant à se verser (FCFA)</label><input id="sal-am" type="number" placeholder="400000" min="1"/></div>
      <div class="fl"><label>Date</label><input id="sal-dt" type="date"/></div>
      <div class="fl"><label>Note</label><input id="sal-nt" placeholder="Ex: Salaire avril 2026…"/></div>
      <div class="ib ib-g"><span>✅</span><div id="sal-calc" style="font-size:13px;"></div></div>
    </div>
    <div class="moa"><button class="btn bo" onclick="cov('ov-sal')">Annuler</button><button class="btn bcy" onclick="savSal()">✅ Confirmer le versement</button></div>
  </div>
</div>

<!-- CONFIRM DELETE -->
<div class="ov" id="ov-conf">
  <div class="cmo">
    <div class="ci">🗑️</div>
    <div class="ct">Confirmer la suppression</div>
    <div class="cd" id="ctxt">Êtes-vous sûr ?</div>
    <div class="flex gi2" style="justify-content:center;">
      <button class="btn bo" onclick="cov('ov-conf')">Annuler</button>
      <button class="btn br2" id="bconf">Supprimer</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>
<script>
// ============================================================
// CONSTANTS
// ============================================================
// Dynamic month system - generates 12 months from a base
const MONTH_NAMES=['Janvier','Février','Mars','Avril','Mai','Juin','Juillet','Août','Septembre','Octobre','Novembre','Décembre'];
function genMonths(baseYear,baseMonth,count){
  const r=[];for(let i=0;i<count;i++){const d=new Date(baseYear,baseMonth+i,1);r.push({label:MONTH_NAMES[d.getMonth()]+' '+d.getFullYear(),key:d.getFullYear()+'-'+String(d.getMonth()+1).padStart(2,'0')});}return r;
}
let MN_BASE={year:2026,month:2}; // March 2026 default
let MN_COUNT=12;
let MNS=genMonths(MN_BASE.year,MN_BASE.month,MN_COUNT);
function MN_label(i){return MNS[i]?.label||'?';}
const MN=['Mars 2026','Avril 2026','Mai 2026','Juin 2026','Juillet 2026','Août 2026','Septembre 2026']; // kept for compat
const DP={don:.10,epargne:.10,invest:.05,dette:.10,conso:.65};
const PC={
  don:{lb:'Plan Don',ic:'🤲',co:'#D97706',bg:'#FEF3C7',bge:'bga'},
  epargne:{lb:'Plan Épargne',ic:'💳',co:'#16A34A',bg:'#DCFCE7',bge:'bgg'},
  invest:{lb:'Plan Investissement',ic:'📈',co:'#2563EB',bg:'#DBEAFE',bge:'bgb'},
  dette:{lb:'Paiement Dettes',ic:'⚠️',co:'#DC2626',bg:'#FEE2E2',bge:'bgr'},
  conso:{lb:'Plan Consommation',ic:'🛒',co:'#7C3AED',bg:'#EDE9FE',bge:'bgp'},
};
const PK=['don','epargne','invest','dette','conso'];
const DI={
  don:['Sadaqa / Aumône','Zakat si applicable','Aides familiales','Solidarité communautaire','Cadeaux famille/amis'],
  epargne:['BOA / Banque','Tontine / AVEC',"Fonds d'urgence",'Objectif moyen terme','Retraite'],
  invest:['Commerce en ligne','Agriculture maraîchère','Aviculture','BRVM / Bourse','Terrain'],
  dette:[],
  conso:['Ravitaillement foyer','Transport','Eau / Électricité','Internet / Mobile','Scolarité','Santé','Habits','Sorties','Imprévus'],
};
const BT={impression:'🖨️',commerce:'🛒',service:'🤝',coaching:'🎯',resto:'🍽️',tech:'💻',agri:'🌱',autre:'📦'};
const CI={loyer:'🏠',energie:'⚡',internet:'📡',salaires:'👥',stock:'📦',transport:'🚗',assurance:'🛡️',autre:'📋'};

// ============================================================
// STATE
// ============================================================
let S=null,mi=0,cp='db',cu=null,stmr=null;
let txTab='tout',exPer='mois',exPl='all';
let eTxId=null,eDtId=null,pDtId=null,undoD=null;
let curMode='perso',curBizId=null,bmi=0,curBizPg='biz-db';
let eCmdId=null,eChgId=null,eFouId=null,cvLines=[];

// ============================================================
// DATA MODELS
// ============================================================
function dM(){return{revenus:[],items:Object.fromEntries(PK.map(k=>[k,(DI[k]||[]).map(n=>({id:uid(),name:n,bp:0,dep:0}))])),dettes:[],depenses:[]};}
function dS(){return{mois:Array.from({length:12},()=>dM()),par:{},businesses:[],objectifs:[],recurrentes:[],mnBase:{year:2026,month:2}};}
function dBM(){return{commandes:[],charges:[],versements:[]};}
function dBiz(nom,type,desc,sal){return{id:uid(),nom,type,desc,salPrev:sal||0,fournisseurs:[],mois:Array.from({length:12},()=>dBM())};}
function M(){return S.mois[mi];}
function B(){return S.businesses?.find(b=>b.id===curBizId)||null;}
function BM(){const b=B();return b?b.mois[bmi]:null;}
function uid(){return Date.now().toString(36)+Math.random().toString(36).slice(2,5);}
function gp2(k){return S?.par?.[k]??DP[k];}

// ============================================================
// AUTH
// ============================================================
window._onUserReady=function(u){cu=u;ldData(u.uid);};
function showScreen(s){
  document.getElementById('ld').style.display='none';
  document.getElementById('au').style.display=s==='auth'?'flex':'none';
  const a=document.getElementById('ap');
  a.style.display=s==='app'?'flex':'none';
  a.classList.toggle('on',s==='app');
}
async function doLogin(){
  const e=g('le'),p=g('lp');
  if(!e){sErr('Email requis');return;}if(!p){sErr('Mot de passe requis');return;}
  const b=document.querySelector('#fl .bpri');bld(b,'Connexion…');
  try{await window._fbReady;await window._fb.signInWithEmailAndPassword(window._fb.auth,e,p);hErr();}
  catch(x){sErr(fE(x.code));bld(b,'Se connecter',false);}
}
async function doRegister(){
  const n=g('rn')||'Utilisateur',e=g('re'),p=g('rp2');
  if(!e){sErr('Email requis');return;}if(!p||p.length<6){sErr('Mot de passe : 6 car. min.');return;}
  const b=document.querySelector('#fr .bpri');bld(b,'Création…');
  try{
    await window._fbReady;
    const{auth,createUserWithEmailAndPassword,updateProfile}=window._fb;
    const c=await createUserWithEmailAndPassword(auth,e,p);
    await updateProfile(c.user,{displayName:n});hErr();
  }catch(x){sErr(fE(x.code));bld(b,'Créer mon compte',false);}
}
async function doLogout(){await window._fbReady;await window._fb.signOut(window._fb.auth);S=null;cu=null;showScreen('auth');}

// ============================================================
// DATA LOAD / PERSIST
// ============================================================
async function ldData(uid2){
  await window._fbReady;const{db,doc,getDoc}=window._fb;
  try{
    const sn=await getDoc(doc(db,'users',uid2));
    S=sn.exists()?JSON.parse(JSON.stringify(sn.data())):dS();
    if(!S.par)S.par={};if(!S.mois)S.mois=[];if(!S.businesses)S.businesses=[];
    while(S.mois.length<12)S.mois.push(dM());
    S.mois.forEach(m=>{
      if(!m.revenus)m.revenus=[];if(!m.depenses)m.depenses=[];if(!m.dettes)m.dettes=[];if(!m.items)m.items={};
      PK.forEach(k=>{
        if(!m.items[k])m.items[k]=[];
        m.items[k].forEach(it=>{
          if(it.budgetPrevu!==undefined&&it.bp===undefined)it.bp=it.budgetPrevu;
          if(it.depense!==undefined&&it.dep===undefined)it.dep=it.depense;
          if(it.bp===undefined)it.bp=0;if(it.dep===undefined)it.dep=0;if(!it.id)it.id=uid();
        });
      });
      m.dettes.forEach(d=>{if(d.paye===undefined)d.paye=0;if(d.total===undefined)d.total=0;if(!d.hist)d.hist=[];if(!d.type)d.type='argent';if(!d.id)d.id=uid();});
    });
    S.businesses.forEach(b=>{
      if(!b.id)b.id=uid();if(!b.fournisseurs)b.fournisseurs=[];if(!b.mois)b.mois=[];
      while(b.mois.length<12)b.mois.push(dBM());
      b.mois.forEach(bm=>{if(!bm.commandes)bm.commandes=[];if(!bm.charges)bm.charges=[];if(!bm.versements)bm.versements=[];});
    });
    if(!S.objectifs)S.objectifs=[];
    if(!S.recurrentes)S.recurrentes=[];
    if(!S.mnBase)S.mnBase={year:2026,month:2};
    MN_BASE=S.mnBase;MNS=genMonths(MN_BASE.year,MN_BASE.month,12);
    populateMnSel();
    if(S.businesses.length>0&&!curBizId)curBizId=S.businesses[0].id;
    const ini=(cu.displayName||cu.email||'?').split(' ').map(w=>w[0]).join('').toUpperCase().slice(0,2);
    document.getElementById('uav').textContent=ini;
    document.getElementById('unm').textContent=cu.displayName||cu.email.split('@')[0];
    ['txdt','pdat','ret-dt','nd-dt','cmd-dt','sal-dt','fou-dt'].forEach(id=>{const e=document.getElementById(id);if(e)e.value=td();});
    showScreen('app');updSB();updBSel();populateMnSel();gp('db');
    if(sn.exists())persist();
  }catch(x){console.error(x);showScreen('auth');}
}
function sSyn(on){['sdo','sdo2','sdo-b'].forEach(id=>document.getElementById(id)?.classList.toggle('sy',on));}
async function persist(){
  if(!cu||!S)return;sSyn(true);
  await window._fbReady;const{db,doc,setDoc}=window._fb;
  try{await setDoc(doc(db,'users',cu.uid),JSON.parse(JSON.stringify(S)));}
  catch(x){toast('Erreur sauvegarde','er');}sSyn(false);
}
function sch(){clearTimeout(stmr);sSyn(true);stmr=setTimeout(persist,1400);}

// ============================================================
// MODE SWITCH
// ============================================================
function setMode(mode){
  curMode=mode;
  const nav=document.getElementById('mnav');
  document.getElementById('mode-perso').classList.toggle('on',mode==='perso');
  document.getElementById('mode-biz').classList.toggle('on',mode==='biz');
  if(mode==='perso'){
    nav.classList.remove('bm');
    document.getElementById('sb').style.display='';document.getElementById('sbb').style.display='none';
    document.getElementById('nbrand').textContent='Xiif XamXam';
    document.querySelectorAll('.pg').forEach(e=>e.classList.remove('on'));
    gp(cp||'db');
  }else{
    nav.classList.add('bm');
    document.getElementById('sb').style.display='none';document.getElementById('sbb').style.display='flex';
    document.getElementById('nbrand').textContent='Business Manager';
    document.querySelectorAll('.pg').forEach(e=>e.classList.remove('on'));
    updBSel();
    if(!S.businesses?.length)gpB('biz-set');else gpB('biz-db');
  }
}
function togSB(){
  if(curMode==='perso')document.getElementById('sb')?.classList.toggle('mob');
  else document.getElementById('sbb')?.classList.toggle('mob');
}

// ============================================================
// PERSO FINANCE CALCS
// ============================================================
function tRev(i){return(S.mois[i].revenus||[]).reduce((s,r)=>s+r.amount,0);}
function gRep(i,k){if(i===0)return 0;return Math.max(0,tRev(i-1)*gp2(k)+gRep(i-1,k)-pDep(i-1,k));}
function bgt(i,k){return tRev(i)*gp2(k)+gRep(i,k);}
function pDep(i,k){
  const m=S.mois[i];
  if(k==='dette')return(m.dettes||[]).reduce((s,d)=>s+d.paye,0);
  return(m.items[k]||[]).reduce((s,it)=>s+it.dep,0)+(m.depenses||[]).filter(d=>d.plan===k).reduce((s,d)=>s+d.amount,0);
}
function aTx(i){
  const m=S.mois[i];
  return[...(m.revenus||[]).map(r=>({...r,type:'revenu',plan:null})),...(m.depenses||[]).map(d=>({...d,type:'depense'}))].sort((a,b)=>(b.date||'').localeCompare(a.date||''));
}

// ============================================================
// NAV PERSO
// ============================================================
function gp(p){
  cp=p;
  document.querySelectorAll('.pg').forEach(e=>e.classList.remove('on'));
  document.querySelectorAll('.ntab,[data-p]').forEach(e=>e.classList.remove('on'));
  document.getElementById('pg-'+p)?.classList.add('on');
  document.querySelector(`[data-p="${p}"]`)?.classList.add('on');
  if(window.innerWidth<=768)document.getElementById('sb')?.classList.remove('mob');
  // Clean stale alert div on every page switch
  document.getElementById('dash-alerts')?.remove();
  rp(p);
}
function chMois(v){mi=parseInt(v);rp(cp);}
function rp(p){
  if(!S)return;
  if(p==='db')rDash();else if(p==='tx')rTx();else if(p==='plans')rPlans();
  else if(['don','epargne','invest','conso'].includes(p))rPS(p);
  else if(p==='dettes')rDettes();else if(p==='export')rExp();else if(p==='param')rParam();
  else if(p==='charts')rCharts();else if(p==='objectifs')rObjectifs();
  else if(p==='recurrentes')rRecurrentes();else if(p==='ia')rIA();
}

// ============================================================
// DASHBOARD PERSO
// ============================================================
function rDash(){
  const rev=tRev(mi),m=M(),t=td();
  document.getElementById('dbtit').textContent='Tableau de bord — '+MNS[mi]?.label||MN[mi]||'';
  const rt=(m.revenus||[]).filter(r=>r.date===t).reduce((s,r)=>s+r.amount,0);
  const dt2=(m.depenses||[]).filter(d=>d.date===t).reduce((s,d)=>s+d.amount,0);
  const tdp=PK.reduce((s,k)=>s+pDep(mi,k),0),sol=rev-tdp;
  // Alert banners for budget overruns
  const alerts=PK.map(k=>{const b2=bgt(mi,k),d2=pDep(mi,k),pct2=b2>0?d2/b2*100:0;return{k,b2,d2,pct2};}).filter(a=>a.pct2>=90&&a.b2>0);
  const alertHtml=alerts.map(a=>`<div class="alert-bar ${a.pct2>=100?'':'warn'}">
    <span class="ab-ico">${a.pct2>=100?'🚨':'⚠️'}</span>
    <span class="ab-txt">${PC[a.k].ic} ${PC[a.k].lb} : ${a.pct2.toFixed(0)}% utilisé (${f(a.d2)} / ${f(a.b2)} FCFA)${a.pct2>=100?' — DÉPASSÉ !':' — Proche de la limite'}</span>
  </div>`).join('');
  if(alertHtml)document.getElementById('dbtit').insertAdjacentHTML('afterend','<div id="dash-alerts" style="margin-top:12px;">'+alertHtml+'</div>');
  document.getElementById('dbkpi').innerHTML=[
    ['Revenus du mois',f(rev),'FCFA','var(--g)'],
    ['Dépenses du mois',f(tdp),'FCFA','var(--r)'],
    ['Solde du mois',f(sol),'FCFA',sol>=0?'var(--g)':'var(--r)'],
    ['Revenus aujourd\'hui',f(rt),'FCFA','var(--g)'],
    ['Dépenses aujourd\'hui',f(dt2),'FCFA','var(--r)'],
    ['Épargne',f(pDep(mi,'epargne')),'FCFA','var(--b)'],
  ].map(([l,v,u,c])=>`<div class="kpi" style="border-left:4px solid ${c}"><div class="kl">${l}</div><div class="kv" style="color:${c}">${v}</div><div class="ks">${u}</div></div>`).join('');
  document.getElementById('dbplans').innerHTML=PK.map(k=>{
    const b=bgt(mi,k),d=pDep(mi,k),pct=b>0?Math.min(100,d/b*100):0,s=b-d;
    return`<div class="pchip" style="background:${PC[k].bg};border-color:${PC[k].co}33" onclick="gp('${k==='dette'?'dettes':k}')">
      <div class="pcn" style="color:${PC[k].co}">${PC[k].ic} ${PC[k].lb} · ${Math.round(gp2(k)*100)}%</div>
      <div class="pcb" style="color:${PC[k].co}">${f(b)} FCFA</div>
      <div class="pr" style="background:${PC[k].co}22;margin-top:7px"><div class="prt" style="width:${pct}%;background:${PC[k].co}"></div></div>
      <div class="pcs" style="color:${PC[k].co}">Solde : ${f(s)} FCFA</div>
    </div>`;
  }).join('');
  const rec=aTx(mi).slice(0,8);
  const el=document.getElementById('dbrec');
  el.innerHTML=rec.length?'<div class="txl">'+rec.map(tx=>txH(tx,false)).join('')+'</div>':'<div class="est"><div class="eico">💳</div><p>Aucune transaction.</p></div>';
  updSB();updSBAlerts();
}

// ============================================================
// TRANSACTIONS
// ============================================================
function stt(t){txTab=t;['tout','rev','dep'].forEach(x=>document.getElementById('tt-'+x)?.classList.toggle('on',x===t));rTx();}
function rTx(){
  const sx=(g('tsx')||'').toLowerCase(),pf=g('tspf');
  let txs=aTx(mi);
  if(txTab==='rev')txs=txs.filter(t=>t.type==='revenu');
  if(txTab==='dep')txs=txs.filter(t=>t.type==='depense');
  if(sx)txs=txs.filter(t=>(t.desc||t.source||'').toLowerCase().includes(sx));
  if(pf)txs=txs.filter(t=>t.plan===pf);
  const el=document.getElementById('txlist');
  el.innerHTML=txs.length?'<div class="txl">'+txs.map(tx=>txH(tx,true)).join('')+'</div>':'<div class="est"><div class="eico">💳</div><p>Aucune transaction.</p></div>';
}
function txH(tx,ac){
  const ir=tx.type==='revenu';
  const ds=es(tx.source||tx.desc||'');
  const pl=tx.plan?`<span class="bge ${PC[tx.plan]?.bge||'bgs'}">${PC[tx.plan]?.ic||''} ${PC[tx.plan]?.lb||tx.plan}</span>`:`<span class="bge bgg">Revenu${tx.isDette?' (dette)':tx.isRet?' (retrait ép.)':tx.isSalBiz?' (salaire biz)':''}</span>`;
  const dc=tx.isRet?'ep':ir?'rv':'dp';
  const ac2=ir?'p':(tx.isRet?'b':'n');
  const btns=ac?`<div class="txac"><button class="bico ed" onclick="oeTx('${tx.id}','${tx.type}')">✏️</button><button class="bico dl" onclick="cDel('${tx.id}','${tx.type}','${es(ds)}')">🗑️</button></div>`:'';
  return`<div class="txi"><div class="txd ${dc}"></div><div class="txinf"><div class="txdsc">${ds}</div><div class="txmt">${tx.date||''} ${tx.subcat?'· '+es(tx.subcat):''} ${pl}${tx.note?' · '+es(tx.note):''}</div></div><div class="txam ${ac2}">${ir?'+':'-'}${f(tx.amount)}</div>${btns}</div>`;
}
function oaTx(type){
  eTxId=null;document.getElementById('txmot').textContent='Nouvelle transaction';
  sv('txtp',type);sv('txds','');sv('txam','');sv('txdt',td());sv('txpl','conso');sv('txnt','');
  upPlan();upSub();document.getElementById('bstx').textContent='Enregistrer';document.getElementById('bstx').disabled=false;
  ov('ov-tx');
}
function oeTx(id,type){
  const m=M();const tx=type==='revenu'?(m.revenus||[]).find(r=>r.id===id):(m.depenses||[]).find(d=>d.id===id);if(!tx)return;
  eTxId=id;document.getElementById('txmot').textContent='Modifier la transaction';
  sv('txtp',type);sv('txds',tx.source||tx.desc||'');sv('txam',tx.amount);sv('txdt',tx.date||td());sv('txpl',tx.plan||'conso');sv('txnt',tx.note||'');
  upPlan();upSub();if(tx.subcat)sv('txsc',tx.subcat);
  document.getElementById('bstx').textContent='Mettre à jour';document.getElementById('bstx').disabled=false;
  ov('ov-tx');
}
function upPlan(){document.getElementById('txpf').style.display=g('txtp')==='revenu'?'none':'block';}
function upSub(){
  const pl=g('txpl'),items=M().items[pl]||[];
  const sl=document.getElementById('txsc');
  sl.innerHTML='<option value="">— Général —</option>'+items.filter(i=>i.name).map(i=>`<option value="${es(i.name)}">${es(i.name)}</option>`).join('');
}
async function savTx(){
  const tp=g('txtp'),ds=g('txds'),am=parseFloat(g('txam')),dt=g('txdt'),pl=g('txpl'),sc=g('txsc'),nt=g('txnt');
  if(!ds){toast('Description requise','er');return;}if(!am||am<1){toast('Montant invalide','er');return;}
  const btn=document.getElementById('bstx');bld(btn,'…');
  const m=M();
  if(eTxId){
    if(tp==='revenu'){const r=(m.revenus||[]).find(x=>x.id===eTxId);if(r)Object.assign(r,{source:ds,amount:am,date:dt,note:nt});}
    else{const old=(m.depenses||[]).find(x=>x.id===eTxId);if(old){if(old.subcat){const it=(m.items[old.plan]||[]).find(i=>i.name===old.subcat);if(it)it.dep=Math.max(0,it.dep-old.amount);}Object.assign(old,{desc:ds,amount:am,date:dt,plan:pl,subcat:sc,note:nt});if(sc){const it=(m.items[pl]||[]).find(i=>i.name===sc);if(it)it.dep+=am;}}}
    toast('Modifié ✓','sg');
  }else{
    if(tp==='revenu')m.revenus.push({id:uid(),source:ds,amount:am,date:dt,note:nt});
    else{m.depenses.push({id:uid(),desc:ds,amount:am,date:dt,plan:pl,subcat:sc,note:nt});if(sc){const it=(m.items[pl]||[]).find(i=>i.name===sc);if(it)it.dep+=am;}}
    toast('Ajouté ✓','sg');
  }
  cov('ov-tx');sch();rp(cp);
}
function cDel(id,type,ds){
  document.getElementById('ctxt').textContent=`Supprimer "${ds}" ?`;
  document.getElementById('bconf').onclick=()=>doDel(id,type);ov('ov-conf');
}
function doDel(id,type){
  const m=M();let del=null;
  if(type==='revenu'){del=JSON.parse(JSON.stringify((m.revenus||[]).find(r=>r.id===id)||{}));m.revenus=m.revenus.filter(r=>r.id!==id);}
  else{const d=(m.depenses||[]).find(x=>x.id===id);if(d){del=JSON.parse(JSON.stringify(d));if(d.subcat){const it=(m.items[d.plan]||[]).find(i=>i.name===d.subcat);if(it)it.dep=Math.max(0,it.dep-d.amount);}m.depenses=m.depenses.filter(x=>x.id!==id);}}
  undoD={del,type};cov('ov-conf');sch();rp(cp);
  toastUndo('Supprimé',()=>{if(undoD){const mm=M();if(undoD.type==='revenu')mm.revenus.push(undoD.del);else{mm.depenses.push(undoD.del);const d=undoD.del;if(d.subcat){const it=(mm.items[d.plan]||[]).find(i=>i.name===d.subcat);if(it)it.dep+=d.amount;}}undoD=null;sch();rp(cp);}});
}

// ============================================================
// PLANS PERSO
// ============================================================
function rPlans(){document.getElementById('plc').innerHTML=PK.map(k=>planCard(k,M())).join('');}
function rPS(k){const el=document.getElementById('plc-'+k);if(!el)return;el.innerHTML=planCard(k,M());updSB();}
function planCard(k,m){
  const b=bgt(mi,k),d=pDep(mi,k),s=b-d,pct=b>0?Math.min(100,d/b*100):0,rep=gRep(mi,k);
  const items=m.items[k]||[];
  const retBtn=k==='epargne'?`<button class="btn bb bsm" onclick="oaRet()">↩ Retrait épargne</button>`:'';
  return`<div class="card mb4" style="border-left:4px solid ${PC[k].co}">
    <div class="flex aic jb mb3" style="flex-wrap:wrap;gap:8px">
      <div style="font-size:15px;font-weight:800">${PC[k].ic} ${PC[k].lb} <span style="color:var(--s4);font-size:12px;font-weight:500">· ${Math.round(gp2(k)*100)}%</span></div>
      <div class="flex gi2 fw aic"><span class="tsm tmu">Budget : <strong>${f(b)} FCFA</strong></span><span class="bge ${s>=0?'bgg':'bgr'}">${s>=0?'+':''}${f(s)} FCFA</span>${retBtn}</div>
    </div>
    <div class="pr" style="background:var(--s1);height:7px;margin-bottom:8px"><div class="prt" style="width:${pct}%;background:${PC[k].co}"></div></div>
    <div style="font-size:12px;color:var(--s5);margin-bottom:12px">${f(d)} FCFA dépensé · ${pct.toFixed(0)}%${rep>0?` · <span style="color:${PC[k].co}">Report +${f(rep)} FCFA</span>`:''}</div>
    <div class="scx"><table class="dt">
      <thead><tr><th>Désignation</th><th style="text-align:right;width:125px">Budget prévu</th><th style="text-align:right;width:125px">Dépensé</th><th style="text-align:right;width:80px">Écart</th><th style="width:30px"></th></tr></thead>
      <tbody>${items.map(it=>{const e=it.bp-it.dep;return`<tr><td><input style="border:none;background:transparent;font-size:13px;color:var(--s8);width:100%;outline:none;font-family:var(--font)" value="${es(it.name)}" onchange="uIN('${k}','${it.id}',this.value)"/></td><td style="text-align:right"><input type="number" min="0" value="${it.bp||''}" placeholder="0" style="border:1.5px solid var(--s2);border-radius:6px;padding:5px 8px;font-size:13px;width:110px;text-align:right;outline:none;font-family:var(--font)" onchange="uIB('${k}','${it.id}',this.value)"/></td><td style="text-align:right"><input type="number" min="0" value="${it.dep||''}" placeholder="0" style="border:1.5px solid var(--s2);border-radius:6px;padding:5px 8px;font-size:13px;width:110px;text-align:right;outline:none;font-family:var(--font);color:${it.dep>0?PC[k].co:'inherit'}" onchange="uID('${k}','${it.id}',this.value)"/></td><td style="text-align:right;font-weight:700;color:${e>=0?'var(--g)':'var(--r)'}">${it.bp>0||it.dep>0?f(e):'—'}</td><td><button class="bico dl" onclick="cDelIt('${k}','${it.id}','${es(it.name)}')">🗑️</button></td></tr>`;}).join('')}</tbody>
      <tfoot><tr><td>TOTAL</td><td style="text-align:right">${f(items.reduce((s,i)=>s+i.bp,0))} FCFA</td><td style="text-align:right;color:${PC[k].co}">${f(d)} FCFA</td><td style="text-align:right;color:${s>=0?'var(--g)':'var(--r)'}">${f(s)} FCFA</td><td></td></tr></tfoot>
    </table></div>
    <button class="btn bo bsm mt3" onclick="aIt('${k}')">+ Ajouter ligne</button>
  </div>`;
}
function uIN(k,id,v){const i=M().items[k].find(x=>x.id===id);if(i)i.name=v;sch();}
function uIB(k,id,v){const i=M().items[k].find(x=>x.id===id);if(i)i.bp=parseFloat(v)||0;sch();rp(cp);}
function uID(k,id,v){const i=M().items[k].find(x=>x.id===id);if(i)i.dep=parseFloat(v)||0;sch();rp(cp);}
function aIt(k){M().items[k].push({id:uid(),name:'Nouvelle ligne',bp:0,dep:0});sch();rp(cp);}
function cDelIt(k,id,nm){document.getElementById('ctxt').textContent=`Supprimer "${nm}" ?`;document.getElementById('bconf').onclick=()=>{M().items[k]=M().items[k].filter(i=>i.id!==id);cov('ov-conf');sch();rp(cp);};ov('ov-conf');}

// ============================================================
// DETTES
// ============================================================
function rDettes(){
  const m=M(),b=bgt(mi,'dette'),rep=gRep(mi,'dette');
  const tp=(m.dettes||[]).reduce((s,d)=>s+d.paye,0),sol=b-tp;
  const tDues=(m.dettes||[]).reduce((s,d)=>s+(d.total-d.paye),0);
  const el=document.getElementById('detc');
  const sum=`<div class="kg mb4">
    <div class="kpi" style="border-left:4px solid var(--r)"><div class="kl">Total dues</div><div class="kv" style="color:var(--r)">${f(tDues)}</div><div class="ks">FCFA à rembourser</div></div>
    <div class="kpi" style="border-left:4px solid var(--am)"><div class="kl">Total contracté</div><div class="kv" style="color:var(--am)">${f((m.dettes||[]).reduce((s,d)=>s+d.total,0))}</div><div class="ks">FCFA</div></div>
    <div class="kpi" style="border-left:4px solid var(--g)"><div class="kl">Total remboursé</div><div class="kv" style="color:var(--g)">${f(tp)}</div><div class="ks">FCFA payés</div></div>
    <div class="kpi" style="border-left:4px solid var(--b)"><div class="kl">Budget ce mois</div><div class="kv" style="color:var(--b)">${f(b)}</div><div class="ks">FCFA</div></div>
    <div class="kpi" style="border-left:4px solid ${sol>=0?'var(--g)':'var(--r)'}"><div class="kl">Solde dispo.</div><div class="kv" style="color:${sol>=0?'var(--g)':'var(--r)'}">${f(sol)}</div><div class="ks">FCFA</div></div>
    <div class="kpi" style="border-left:4px solid var(--pu)"><div class="kl">Nb. dettes</div><div class="kv" style="color:var(--pu)">${(m.dettes||[]).length}</div><div class="ks">${(m.dettes||[]).filter(d=>d.total-d.paye<=0).length} soldée(s)</div></div>
  </div>`;
  if(!(m.dettes||[]).length){el.innerHTML=sum+`<div class="est"><div class="eico">✅</div><p>Aucune dette pour ${MNS[mi]?.label||'ce mois'}.</p></div>`;return;}
  el.innerHTML=sum+`<div class="dgrid">${(m.dettes||[]).map(d=>{
    const rs=d.total-d.paye,pp=d.total>0?Math.min(100,d.paye/d.total*100):0;
    return`<div class="dc ${pp>=100?'ok':''}">
      <div class="flex aic jb mb3"><div><div class="dcn">${es(d.nom)}</div><div class="flex gi2 mt2"><span class="bge ${d.type==='nature'?'bga':'bgb'}">${d.type==='nature'?'📦 Nature':'💰 Argent'}</span></div></div>
      <div class="flex gi2"><button class="bico ed" onclick="oDet('${d.id}')">✏️</button><button class="bico dl" onclick="cDelD('${d.id}','${es(d.nom)}')">🗑️</button></div></div>
      <div class="dr"><span class="dl2">Montant total</span><span class="dv2">${f(d.total)} FCFA</span></div>
      <div class="dr"><span class="dl2">Remboursé</span><span class="dv2 tg">${f(d.paye)} FCFA</span></div>
      <div class="dr"><span class="dl2">Reste dû</span><span class="dv2 tr" style="font-size:15px">${f(rs)} FCFA</span></div>
      ${d.dlim?`<div class="dr"><span class="dl2">Échéance</span><span class="dv2" style="color:${new Date(d.dlim)<new Date()?'var(--r)':'var(--am)'};">${d.dlim}</span></div>`:''}
      ${d.mens>0?`<div class="dr"><span class="dl2">Mensualité</span><span class="dv2">${f(d.mens)} FCFA</span></div>`:''}
      ${d.note?`<div class="txs tmu mt2">📝 ${es(d.note)}</div>`:''}
      <div class="mt3"><div class="flex jb txs tmu mb3"><span>Progression</span><span>${pp.toFixed(0)}%</span></div>
      <div class="pr" style="background:var(--s1);height:7px"><div class="prt" style="width:${pp}%;background:${pp>=100?'var(--g)':'var(--r)'}"></div></div></div>
      ${pp>=100?'<div class="tsm fb tg mt3" style="text-align:center">✅ Soldée !</div>':`<button class="btn bg bsm mt3" style="width:100%;justify-content:center" onclick="oPaie('${d.id}')">💸 Enregistrer paiement</button>`}
    </div>`;
  }).join('')}</div>`;
}
function oaND(){sv('nd-tp','argent');sv('nd-cr','');sv('nd-mt','');sv('nd-ds','');sv('nd-vl','');sv('nd-dt',td());sv('nd-dl','');sv('nd-mn','0');sv('nd-nt','');document.getElementById('nd-ar').checked=true;eDtId=null;updNDT();ov('ov-nd');}
function updNDT(){const isN=g('nd-tp')==='nature';document.getElementById('nd-mf').style.display=isN?'none':'block';document.getElementById('nd-nf').style.display=isN?'block':'none';document.getElementById('nd-vf').style.display=isN?'block':'none';document.getElementById('nd-ro').style.display=isN?'none':'flex';}
async function savND(){
  const tp=g('nd-tp'),crea=g('nd-cr'),mont=parseFloat(g('nd-mt'))||0,desc=g('nd-ds'),val=parseFloat(g('nd-vl'))||0;
  const date=g('nd-dt'),dlim=g('nd-dl'),mens=parseFloat(g('nd-mn'))||0,note=g('nd-nt');
  const addR=document.getElementById('nd-ar')?.checked;
  if(!crea){toast('Nom du créancier requis','er');return;}
  if(tp==='argent'&&(!mont||mont<1)){toast('Montant requis','er');return;}
  if(tp==='nature'&&!desc){toast('Description requise','er');return;}
  const total=tp==='argent'?mont:val;const m=M();if(!m.dettes)m.dettes=[];
  m.dettes.push({id:uid(),nom:crea,type:tp,desc:tp==='nature'?desc:'',total,paye:0,dlim,mens,note,hist:[]});
  if(tp==='argent'&&addR&&mont>0){m.revenus.push({id:uid(),source:'Dette reçue — '+crea,amount:mont,date,note:'Debt: '+note,isDette:true});toast('Dette + '+f(mont)+' FCFA aux revenus ✓','sg');}
  else toast('Dette enregistrée ✓','sg');
  cov('ov-nd');sch();gp('dettes');
}
function oDet(id){eDtId=id;const d=M().dettes.find(x=>x.id===id);if(!d)return;sv('de-nm',d.nom);sv('de-tp',d.type||'argent');sv('de-ds',d.desc||'');sv('de-tt',d.total);sv('de-py',d.paye);sv('de-dl',d.dlim||'');sv('de-mn',d.mens);sv('de-nt',d.note||'');ov('ov-det');}
async function savDet(){
  const nm=g('de-nm'),tp=g('de-tp'),desc=g('de-ds'),tot=parseFloat(g('de-tt'))||0,pay=parseFloat(g('de-py'))||0,dlim=g('de-dl'),men=parseFloat(g('de-mn'))||0,not=g('de-nt');
  if(!nm||!tot){toast('Nom et montant requis','er');return;}
  const m=M();if(!m.dettes)m.dettes=[];
  if(eDtId){const d=m.dettes.find(x=>x.id===eDtId);if(d)Object.assign(d,{nom:nm,type:tp,desc,total:tot,paye:pay,dlim,mens:men,note:not});}
  else m.dettes.push({id:uid(),nom:nm,type:tp,desc,total:tot,paye:pay,dlim,mens:men,note:not,hist:[]});
  cov('ov-det');sch();rDettes();toast('Créancier enregistré ✓','sg');
}
function cDelD(id,nm){document.getElementById('ctxt').textContent=`Supprimer la dette "${nm}" ?`;document.getElementById('bconf').onclick=()=>{M().dettes=M().dettes.filter(d=>d.id!==id);cov('ov-conf');sch();rDettes();};ov('ov-conf');}
function oPaie(id){pDtId=id;const d=M().dettes.find(x=>x.id===id);if(!d)return;document.getElementById('pinfo').innerHTML=`<strong>${es(d.nom)}</strong> — Reste : <strong style="color:var(--r)">${f(d.total-d.paye)} FCFA</strong>`;sv('pamt',d.mens||'');sv('pdat',td());ov('ov-paie');}
async function savPaie(){
  const am=parseFloat(g('pamt'))||0,dt=g('pdat');if(!am){toast('Montant requis','er');return;}
  const d=M().dettes.find(x=>x.id===pDtId);if(!d)return;
  d.paye=Math.min(d.total,d.paye+am);d.hist=d.hist||[];d.hist.push({date:dt,amount:am});
  M().depenses.push({id:uid(),desc:'Remboursement — '+d.nom,amount:am,date:dt,plan:'dette',subcat:d.nom,isDettePaie:true});
  cov('ov-paie');sch();rDettes();toast('Paiement '+f(am)+' FCFA enregistré ✓','sg');
}

// ============================================================
// RETRAIT ÉPARGNE
// ============================================================
function oaRet(){
  const sol=bgt(mi,'epargne')-pDep(mi,'epargne');
  document.getElementById('ret-info').innerHTML='💳 Épargne disponible ce mois : <strong>'+f(Math.max(0,sol))+' FCFA</strong>';
  sv('ret-am','');sv('ret-dt',td());sv('ret-mo','');ov('ov-ret');
}
async function savRet(){
  const am=parseFloat(g('ret-am'))||0,date=g('ret-dt'),motif=g('ret-mo')||'Retrait épargne';
  if(!am||am<1){toast('Montant requis','er');return;}
  const sol=bgt(mi,'epargne')-pDep(mi,'epargne');
  if(am>Math.max(0,sol)){toast('Dépasse l\'épargne disponible ('+f(Math.max(0,sol))+' FCFA)','er');return;}
  const m=M();
  m.revenus.push({id:uid(),source:'Retrait épargne — '+motif,amount:am,date,isRet:true});
  m.depenses.push({id:uid(),desc:'Retrait épargne — '+motif,amount:am,date,plan:'epargne',isRet:true});
  cov('ov-ret');sch();toast('Retrait '+f(am)+' FCFA ajouté aux revenus ✓','sg');rp(cp);
}

// ============================================================
// EXPORT
// ============================================================
function sePer(el,v){exPer=v;document.querySelectorAll('#efp .ec').forEach(c=>c.classList.remove('on'));el.classList.add('on');rExp();}
function sePlan(el,v){exPl=v;document.querySelectorAll('#efpl .ec').forEach(c=>c.classList.remove('on'));el.classList.add('on');rExp();}
function exRows(){
  let ms=[];
  if(exPer==='mois')ms=[mi];else if(exPer==='3mois')ms=Array.from({length:3},(_,i)=>Math.max(0,mi-2+i));
  else if(exPer==='6mois')ms=Array.from({length:6},(_,i)=>Math.max(0,mi-5+i));else ms=Array.from({length:S.mois.length},(_,i)=>i);
  ms=[...new Set(ms)].filter(x=>x>=0&&x<7);
  const rows=[];
  ms.forEach(x=>{
    const m=S.mois[x];
    if(exPl==='all')(m.revenus||[]).forEach(r=>rows.push({Date:r.date,Mois:MN[x],Type:'Revenu',Montant:r.amount,Plan:'—',Libellé:r.source||'',Note:r.note||''}));
    (m.depenses||[]).filter(d=>exPl==='all'||d.plan===exPl).forEach(d=>rows.push({Date:d.date,Mois:MN[x],Type:'Dépense',Montant:-d.amount,Plan:PC[d.plan]?.lb||d.plan,Libellé:d.desc||'',Note:d.note||''}));
  });
  return rows.sort((a,b)=>(b.Date||'').localeCompare(a.Date||''));
}
function rExp(){
  const rows=exRows(),el=document.getElementById('exprev');
  if(!rows.length){el.innerHTML='<div class="est"><div class="eico">📋</div><p>Aucune donnée.</p></div>';return;}
  const tot=rows.reduce((s,r)=>s+r.Montant,0);
  el.innerHTML=`<div class="tsm tmu mb3">${rows.length} transactions · Solde : <strong style="color:${tot>=0?'var(--g)':'var(--r)'}">${f(tot)} FCFA</strong></div>
  <div class="scx"><table class="dt"><thead><tr><th>Date</th><th>Type</th><th>Libellé</th><th>Plan</th><th style="text-align:right">Montant</th></tr></thead>
  <tbody>${rows.slice(0,10).map(r=>`<tr><td>${r.Date||''}</td><td><span class="bge ${r.Type==='Revenu'?'bgg':'bgr'}">${r.Type}</span></td><td>${es(r.Libellé)}</td><td class="tmu">${es(r.Plan)}</td><td style="text-align:right;font-weight:700;color:${r.Montant>=0?'var(--g)':'var(--r)'}">${f(r.Montant)}</td></tr>`).join('')}</tbody></table></div>
  ${rows.length>10?`<div class="txs tmu mt2">… et ${rows.length-10} autres dans le fichier</div>`:''}`;
}
function doExp(){
  const rows=exRows();if(!rows.length){toast('Aucune donnée','er');return;}
  const ws=XLSX.utils.json_to_sheet(rows);
  const wb=XLSX.utils.book_new();XLSX.utils.book_append_sheet(wb,ws,'Transactions');
  const sr=MNS.map((_,i)=>{const rv=tRev(i),dp=PK.reduce((s,k)=>s+pDep(i,k),0);return{Mois:MN[i],Revenus:rv,Dépenses:dp,Solde:rv-dp,...Object.fromEntries(PK.map(k=>[PC[k].lb,pDep(i,k)]))};});
  XLSX.utils.book_append_sheet(wb,XLSX.utils.json_to_sheet(sr),'Synthèse');
  XLSX.writeFile(wb,'XiifXamXam_'+td()+'.xlsx');toast('Export téléchargé ✓','sg');
}

// ============================================================
// PARAM
// ============================================================
function rParam(){
  const tot=Math.round(PK.reduce((s,k)=>s+gp2(k)*100,0));const ok=tot===100;
  document.getElementById('parc').innerHTML=`
  ${PK.map(k=>{const p=Math.round(gp2(k)*100);return`<div class="pp"><div class="pph"><div class="ppn" style="color:${PC[k].co}">${PC[k].ic} ${PC[k].lb}</div><div class="flex aic gi2"><input class="ppi" id="pp-${k}" type="number" min="0" max="100" value="${p}" oninput="spR('${k}',this.value)" style="border-color:${PC[k].co}33"/><span class="tmu tsm">%</span></div></div><input type="range" min="0" max="100" value="${p}" id="pr-${k}" style="accent-color:${PC[k].co}" oninput="spI('${k}',this.value)"/><div class="txs tmu mt2">Pour 1 000 000 FCFA → <strong style="color:${PC[k].co}">${f(gp2(k)*1e6)} FCFA</strong></div></div>`;}).join('')}
  <div id="ptot" style="border-radius:var(--r12);padding:13px 16px;background:${ok?'var(--gl)':'var(--rl)'};border:1.5px solid ${ok?'var(--gm)':'var(--rm)'};margin-bottom:14px;display:flex;align-items:center;justify-content:space-between;">
    <span style="font-size:13px;font-weight:700;color:${ok?'var(--g)':'var(--r)'}">Total : <span id="ptv">${tot}</span>%</span>
    <span style="font-size:12px;font-weight:700;color:${ok?'var(--g)':'var(--r)'}" id="pst">${ok?'✅ Parfait !':'⚠️ Doit être 100%'}</span>
  </div>
  <div class="flex gi2 fw"><button class="btn bg" onclick="savPar()">💾 Enregistrer</button><button class="btn bo" onclick="resPar()">↺ Réinitialiser</button></div>`;
}
function spR(k,v){const r=document.getElementById('pr-'+k);if(r)r.value=v;upPT();}
function spI(k,v){const i=document.getElementById('pp-'+k);if(i)i.value=v;upPT();}
function upPT(){
  const tot=PK.reduce((s,k)=>s+(parseInt(document.getElementById('pp-'+k)?.value)||0),0);const ok=tot===100;
  const tb=document.getElementById('ptot'),tv=document.getElementById('ptv'),st=document.getElementById('pst');
  if(tv)tv.textContent=tot;if(st){st.textContent=ok?'✅ Parfait !':'⚠️ Doit être 100%';st.style.color=ok?'var(--g)':'var(--r)';}
  if(tb){tb.style.background=ok?'var(--gl)':'var(--rl)';tb.style.borderColor=ok?'var(--gm)':'var(--rm)';}
}
function savPar(){
  const tot=PK.reduce((s,k)=>s+(parseInt(document.getElementById('pp-'+k)?.value)||0),0);
  if(tot!==100){toast('Le total doit être 100%','er');return;}
  if(!S.par)S.par={};PK.forEach(k=>{S.par[k]=(parseInt(document.getElementById('pp-'+k)?.value)||0)/100;});
  sch();toast('Paramètres enregistrés ✓','sg');rParam();updSB();
}
function resPar(){if(!S.par)S.par={};PK.forEach(k=>{S.par[k]=DP[k];});sch();toast('Réinitialisé ✓','sg');rParam();updSB();}
function updSB(){if(!S)return;const map={don:'sb-don',epargne:'sb-ep',invest:'sb-inv',dette:'sb-det',conso:'sb-con'};PK.forEach(k=>{const el=document.getElementById(map[k]);if(el)el.textContent=Math.round(gp2(k)*100)+'%';});}

// ============================================================
// BUSINESS - HELPERS
// ============================================================
function updBSel(){
  const sel=document.getElementById('bsel');if(!sel)return;
  sel.innerHTML=(S.businesses||[]).map(b=>`<option value="${b.id}"${b.id===curBizId?' selected':''}>${BT[b.type]||'📦'} ${es(b.nom)}</option>`).join('');
  if(!S.businesses?.length)sel.innerHTML='<option value="">— Aucun business —</option>';
}
function chBiz(id){curBizId=id;rpB(curBizPg);}
function chMoisB(v){bmi=parseInt(v);rpB(curBizPg);}
function getBi(){return(S.businesses||[]).findIndex(b=>b.id===curBizId);}
function gpB(p){
  curBizPg=p;
  document.querySelectorAll('.pg').forEach(e=>e.classList.remove('on'));
  document.querySelectorAll('[data-bp]').forEach(e=>e.classList.remove('on'));
  document.getElementById('pg-'+p)?.classList.add('on');
  document.querySelector(`[data-bp="${p}"]`)?.classList.add('on');
  if(window.innerWidth<=768)document.getElementById('sbb')?.classList.remove('mob');
  rpB(p);
}
function rpB(p){
  if(!S)return;
  if(p==='biz-db')rBDB();else if(p==='biz-cmd')rBCmd();else if(p==='biz-charges')rBChg();
  else if(p==='biz-fou')rBFou();else if(p==='biz-sal')rBSal();else if(p==='biz-set')rBSet();
  else if(p==='biz-factures')rBFactures();
}

// BIZ CALC HELPERS
function bCA(bi,i){const b=S.businesses[bi];if(!b)return 0;return(b.mois[i]?.commandes||[]).filter(c=>c.statut==='termine').reduce((s,c)=>s+c.px,0);}
function bCV(bi,i){const b=S.businesses[bi];if(!b)return 0;return(b.mois[i]?.commandes||[]).filter(c=>c.statut!=='annule').reduce((s,c)=>s+(c.cvs||[]).reduce((cs,cv)=>cs+cv.mt,0),0);}
function bCF(bi,i){const b=S.businesses[bi];if(!b)return 0;return(b.mois[i]?.charges||[]).reduce((s,c)=>{if(c.fr==='mensuel')return s+c.mt;if(c.fr==='trimestriel')return s+c.mt/3;if(c.fr==='annuel')return s+c.mt/12;return s+c.mt;},0);}
function bBen(bi,i){return bCA(bi,i)-bCV(bi,i)-bCF(bi,i);}
function bVers(bi,i){const b=S.businesses[bi];if(!b)return 0;return(b.mois[i]?.versements||[]).reduce((s,v)=>s+v.mt,0);}

// ============================================================
// BIZ DASHBOARD
// ============================================================
function rBDB(){
  const el=document.getElementById('bdb');
  if(!S.businesses?.length){el.innerHTML=`<div class="est"><div class="eico">🏢</div><p style="font-size:15px;font-weight:700;">Aucun business configuré</p><p class="txs tmu mt2">Créez votre premier business pour commencer.</p><button class="btn bcy mt4" onclick="gpB('biz-set')">🏢 Créer mon premier business</button></div>`;return;}
  const bi=getBi();if(bi<0){el.innerHTML='<p class="tmu">Sélectionnez un business.</p>';return;}
  const b=S.businesses[bi];
  const ca=bCA(bi,bmi),cv=bCV(bi,bmi),cf=bCF(bi,bmi),ben=bBen(bi,bmi),vers=bVers(bi,bmi);
  const marge=ca>0?((ben/ca)*100).toFixed(1):0;
  const nbCmd=(b.mois[bmi]?.commandes||[]).length;
  const nbEC=(b.mois[bmi]?.commandes||[]).filter(c=>c.statut==='en-cours').length;
  el.innerHTML=`
    <div class="biz-hdr">
      <div style="font-size:12px;font-weight:700;color:#64748B;text-transform:uppercase;letter-spacing:.5px;margin-bottom:4px;">${BT[b.type]||'📦'} ${es(b.nom)}</div>
      <div style="font-size:22px;font-weight:800;margin-bottom:4px;">Dashboard — ${MNS[bmi]?.label||MN[bmi]||''}</div>
      <div style="font-size:13px;color:#94A3B8;">${es(b.desc||'')}</div>
    </div>
    <div class="kg">
      <div class="kpi" style="border-left:4px solid var(--cy)"><div class="kl">Chiffre d'affaires</div><div class="kv" style="color:var(--cy)">${f(ca)}</div><div class="ks">FCFA (cmds terminées)</div></div>
      <div class="kpi" style="border-left:4px solid var(--r)"><div class="kl">Coûts variables</div><div class="kv" style="color:var(--r)">${f(cv)}</div><div class="ks">FCFA matières premières</div></div>
      <div class="kpi" style="border-left:4px solid var(--am)"><div class="kl">Charges fixes</div><div class="kv" style="color:var(--am)">${f(cf)}</div><div class="ks">FCFA ce mois</div></div>
      <div class="kpi" style="border-left:4px solid ${ben>=0?'var(--g)':'var(--r)'}"><div class="kl">Bénéfice net</div><div class="kv" style="color:${ben>=0?'var(--g)':'var(--r)'}">${f(ben)}</div><div class="ks">FCFA · Marge ${marge}%</div></div>
      <div class="kpi" style="border-left:4px solid var(--pu)"><div class="kl">Salaire versé</div><div class="kv" style="color:var(--pu)">${f(vers)}</div><div class="ks">FCFA · Prévu : ${f(b.salPrev||0)}</div></div>
      <div class="kpi" style="border-left:4px solid var(--b)"><div class="kl">Commandes</div><div class="kv" style="color:var(--b)">${nbCmd}</div><div class="ks">${nbEC} en cours</div></div>
    </div>
    <div class="sal-bn">
      <div><div class="sl">💰 Salaire dirigeant — ${MNS[bmi]?.label||MN[bmi]||''}</div><div class="sa">${f(vers)} FCFA versés</div><div class="ss">Prévu : ${f(b.salPrev||0)} FCFA · Bénéfice net : ${f(ben)} FCFA</div></div>
      <button class="sal-bt" onclick="oSal()">💸 Se verser son salaire →</button>
    </div>
    <div class="card">
      <div class="flex aic jb mb3"><div style="font-size:14px;font-weight:700;">Commandes récentes</div><button class="btn bo bsm" onclick="gpB('biz-cmd')">Voir tout →</button></div>
      ${renderCmds((b.mois[bmi]?.commandes||[]).slice(-5).reverse(),false)}
    </div>`;
}

// ============================================================
// BIZ COMMANDES
// ============================================================
let cmdTab='tout';
function sttC(t){cmdTab=t;['tout','en-cours','termine','annule'].forEach(x=>{document.getElementById('ct-'+x)?.classList.toggle('on',x===t);});rBCmd();}
function rBCmd(){
  const bi=getBi();if(bi<0){document.getElementById('cmdlist').innerHTML='<div class="est"><div class="eico">📋</div><p>Sélectionnez un business.</p></div>';return;}
  const b=S.businesses[bi];
  let cmds=(b.mois[bmi]?.commandes||[]).slice().reverse();
  if(cmdTab!=='tout')cmds=cmds.filter(c=>c.statut===cmdTab);
  document.getElementById('cmdlist').innerHTML=renderCmds(cmds,true);
}
function renderCmds(cmds,ac){
  if(!cmds.length)return'<div class="est"><div class="eico">📋</div><p>Aucune commande.</p></div>';
  const smap={'en-cours':'⏳ En cours','termine':'✅ Terminée','annule':'❌ Annulée'};
  return cmds.map(c=>{
    const cv=(c.cvs||[]).reduce((s,v)=>s+v.mt,0);
    const ben=c.statut==='termine'?c.px-cv:null;
    const btns=ac?`<div class="flex gi2 mt3"><button class="btn bo bsm" onclick="oEditCmd('${c.id}')">✏️ Modifier</button><button class="btn br2 bsm" onclick="cDelCmd('${c.id}','${es(c.cl)}')">🗑️</button></div>`:'';
    return`<div class="cmd-card">
      <div class="flex aic jb"><div style="flex:1;"><div class="cmd-cl">👤 ${es(c.cl)}</div><div class="cmd-ds">${es(c.ds)}</div><div class="txs tmu mt2">📅 ${c.dt||''} ${c.dlv?'→ livraison : '+c.dlv:''}</div></div>
      <span class="cmd-st ${c.statut||'en-cours'}">${smap[c.statut]||c.statut}</span></div>
      <div class="cmd-fin">
        <div class="cfi"><div class="cfl">Prix vente</div><div class="cfv" style="color:var(--cy)">${f(c.px)} F</div></div>
        <div class="cfi"><div class="cfl">Coûts var.</div><div class="cfv" style="color:var(--r)">${f(cv)} F</div></div>
        <div class="cfi"><div class="cfl">Bénéfice</div><div class="cfv" style="color:${ben===null?'var(--s4)':ben>=0?'var(--g)':'var(--r)'}">${ben===null?'—':f(ben)+' F'}</div></div>
      </div>
      ${(c.cvs||[]).length?`<div class="mt2 txs tmu">Matières : ${(c.cvs||[]).map(cv=>`${es(cv.nm)} (${f(cv.mt)} F)`).join(' · ')}</div>`:''}
      ${c.nt?`<div class="txs tmu mt2">📝 ${es(c.nt)}</div>`:''}${btns}
    </div>`;
  }).join('');
}
function oNewCmd(){
  eCmdId=null;cvLines=[];
  document.getElementById('cmd-mot').textContent='📋 Nouvelle commande';
  sv('cmd-cl','');sv('cmd-ds','');sv('cmd-dt',td());sv('cmd-dl','');sv('cmd-px','0');sv('cmd-st','en-cours');sv('cmd-nt','');
  document.getElementById('cv-lines').innerHTML='';document.getElementById('cmd-calc').style.display='none';
  addCvLine();document.getElementById('bscmd').textContent='Enregistrer la commande';
  ov('ov-cmd');
}
function oEditCmd(id){
  const bi=getBi();if(bi<0)return;const b=S.businesses[bi];
  const c=(b.mois[bmi]?.commandes||[]).find(x=>x.id===id);if(!c)return;
  eCmdId=id;cvLines=JSON.parse(JSON.stringify(c.cvs||[]));
  document.getElementById('cmd-mot').textContent='✏️ Modifier la commande';
  sv('cmd-cl',c.cl);sv('cmd-ds',c.ds);sv('cmd-dt',c.dt||td());sv('cmd-dl',c.dlv||'');sv('cmd-px',c.px);sv('cmd-st',c.statut||'en-cours');sv('cmd-nt',c.nt||'');
  renderCvLines();updCmdCalc();document.getElementById('bscmd').textContent='Mettre à jour';
  ov('ov-cmd');
}
function addCvLine(){cvLines.push({id:uid(),nm:'',mt:0});renderCvLines();}
function renderCvLines(){
  document.getElementById('cv-lines').innerHTML=cvLines.map((cv,i)=>`
    <div style="display:flex;gap:7px;align-items:center;">
      <input style="flex:1;border:1.5px solid var(--s2);border-radius:var(--r8);padding:8px 10px;font-size:13px;outline:none;font-family:var(--font);" placeholder="Ex: Tee-shirts blancs, Encre…" value="${es(cv.nm)}" oninput="cvLines[${i}].nm=this.value;updCmdCalc()"/>
      <input type="number" min="0" style="width:110px;border:1.5px solid var(--s2);border-radius:var(--r8);padding:8px 10px;font-size:13px;outline:none;font-family:var(--font);text-align:right;" placeholder="Montant F" value="${cv.mt||''}" oninput="cvLines[${i}].mt=parseFloat(this.value)||0;updCmdCalc()"/>
      <button class="bico dl" onclick="cvLines.splice(${i},1);renderCvLines();updCmdCalc()">🗑️</button>
    </div>`).join('');
}
function updCmdCalc(){
  const px=parseFloat(g('cmd-px'))||0,tCV=cvLines.reduce((s,cv)=>s+cv.mt,0),ben=px-tCV;
  const box=document.getElementById('cmd-calc'),txt=document.getElementById('cmd-calc-txt');
  if(px>0||tCV>0){box.style.display='flex';txt.innerHTML=`<strong>Récapitulatif :</strong><br>Prix de vente : <strong>${f(px)} FCFA</strong> · Coûts variables : <strong style="color:var(--r)">${f(tCV)} FCFA</strong><br>Bénéfice estimé : <strong style="color:${ben>=0?'var(--g)':'var(--r)'}">${f(ben)} FCFA</strong>${px>0?` (marge ${(ben/px*100).toFixed(1)}%)`:''}`;
  }else box.style.display='none';
}
async function savCmd(){
  const cl=g('cmd-cl'),ds=g('cmd-ds'),dt=g('cmd-dt'),dlv=g('cmd-dl'),px=parseFloat(g('cmd-px'))||0,statut=g('cmd-st'),nt=g('cmd-nt');
  if(!cl){toast('Nom du client requis','er');return;}if(!ds){toast('Description requise','er');return;}
  const bi=getBi();if(bi<0){toast('Sélectionnez un business','er');return;}
  const b=S.businesses[bi];if(!b.mois[bmi])b.mois[bmi]=dBM();
  const cvs=cvLines.filter(c=>c.nm||c.mt>0).map(c=>({id:c.id||uid(),nm:c.nm,mt:c.mt||0}));
  if(eCmdId){const c=(b.mois[bmi].commandes||[]).find(x=>x.id===eCmdId);if(c)Object.assign(c,{cl,ds,dt,dlv,px,statut,nt,cvs});toast('Commande modifiée ✓','sg');}
  else{if(!b.mois[bmi].commandes)b.mois[bmi].commandes=[];b.mois[bmi].commandes.push({id:uid(),cl,ds,dt,dlv,px,statut,nt,cvs});toast('Commande ajoutée ✓','sg');}
  cov('ov-cmd');sch();rpB(curBizPg);
}
function cDelCmd(id,cl){
  document.getElementById('ctxt').textContent=`Supprimer la commande de "${cl}" ?`;
  document.getElementById('bconf').onclick=()=>{const bi=getBi();if(bi<0)return;S.businesses[bi].mois[bmi].commandes=S.businesses[bi].mois[bmi].commandes.filter(c=>c.id!==id);cov('ov-conf');sch();rpB(curBizPg);};ov('ov-conf');
}

// ============================================================
// BIZ CHARGES
// ============================================================
function rBChg(){
  const el=document.getElementById('chglist');const bi=getBi();
  if(bi<0){el.innerHTML='<div class="est"><div class="eico">💸</div><p>Sélectionnez un business.</p></div>';return;}
  const b=S.businesses[bi];const charges=b.mois[bmi]?.charges||[];const total=bCF(bi,bmi);
  if(!charges.length){el.innerHTML=`<div class="ib ib-cy mb4"><span>ℹ️</span><span>Les charges fixes (loyer, électricité, salaires…) sont les coûts récurrents de votre business indépendamment des commandes.</span></div><div class="est"><div class="eico">💸</div><p>Aucune charge fixe pour ${MNS[bmi]?.label||'ce mois'}.</p><button class="btn bcy mt3" onclick="oNewCharge()">+ Ajouter la première charge</button></div>`;return;}
  el.innerHTML=`<div class="kpi mb4" style="border-left:4px solid var(--am);max-width:280px;"><div class="kl">Total charges fixes ce mois</div><div class="kv" style="color:var(--am)">${f(total)}</div><div class="ks">FCFA</div></div>
  <div class="chg-g">${charges.map(c=>`<div class="chg-i">
    <div class="flex aic jb"><div><div style="font-size:13px;font-weight:700;">${CI[c.ct]||'📋'} ${es(c.nm)}</div><div class="txs tmu mt2"><span class="bge bga">${c.fr||'mensuel'}</span></div></div>
    <div class="flex gi2"><button class="bico ed" onclick="oEditChg('${c.id}')">✏️</button><button class="bico dl" onclick="cDelChg('${c.id}','${es(c.nm)}')">🗑️</button></div></div>
    <div style="font-size:18px;font-weight:800;color:var(--r);margin-top:4px;">${f(c.mt)} FCFA</div>
    ${c.nt?`<div class="txs tmu mt2">${es(c.nt)}</div>`:''}
  </div>`).join('')}</div>`;
}
function oNewCharge(){eChgId=null;document.getElementById('chg-mot').textContent='💸 Nouvelle charge fixe';sv('chg-nm','');sv('chg-am','');sv('chg-ct','loyer');sv('chg-fr','mensuel');sv('chg-nt','');document.getElementById('bschg').textContent='Enregistrer';ov('ov-chg');}
function oEditChg(id){const bi=getBi();if(bi<0)return;const b=S.businesses[bi];const c=(b.mois[bmi]?.charges||[]).find(x=>x.id===id);if(!c)return;eChgId=id;document.getElementById('chg-mot').textContent='✏️ Modifier la charge';sv('chg-nm',c.nm);sv('chg-am',c.mt);sv('chg-ct',c.ct);sv('chg-fr',c.fr);sv('chg-nt',c.nt||'');document.getElementById('bschg').textContent='Mettre à jour';ov('ov-chg');}
async function savChg(){
  const nm=g('chg-nm'),amt=parseFloat(g('chg-am'))||0,ct=g('chg-ct'),fr=g('chg-fr'),nt=g('chg-nt');
  if(!nm){toast('Désignation requise','er');return;}if(!amt){toast('Montant requis','er');return;}
  const bi=getBi();if(bi<0)return;const b=S.businesses[bi];
  if(!b.mois[bmi])b.mois[bmi]=dBM();if(!b.mois[bmi].charges)b.mois[bmi].charges=[];
  if(eChgId){const c=b.mois[bmi].charges.find(x=>x.id===eChgId);if(c)Object.assign(c,{nm,mt:amt,ct,fr,nt});}
  else b.mois[bmi].charges.push({id:uid(),nm,mt:amt,ct,fr,nt});
  cov('ov-chg');sch();rpB(curBizPg);toast('Charge enregistrée ✓','sg');
}
function cDelChg(id,nm){document.getElementById('ctxt').textContent=`Supprimer "${nm}" ?`;document.getElementById('bconf').onclick=()=>{const bi=getBi();if(bi<0)return;S.businesses[bi].mois[bmi].charges=S.businesses[bi].mois[bmi].charges.filter(c=>c.id!==id);cov('ov-conf');sch();rpB(curBizPg);};ov('ov-conf');}

// ============================================================
// BIZ FOURNISSEURS
// ============================================================
function rBFou(){
  const el=document.getElementById('foulist');const bi=getBi();
  if(bi<0){el.innerHTML='<div class="est"><div class="eico">🏭</div><p>Sélectionnez un business.</p></div>';return;}
  const b=S.businesses[bi];const fous=b.fournisseurs||[];
  if(!fous.length){el.innerHTML=`<div class="ib ib-cy mb4"><span>ℹ️</span><span>Gérez ici vos fournisseurs de matières premières et stocks. Ils seront disponibles dans les coûts variables de vos commandes.</span></div><div class="est"><div class="eico">🏭</div><p>Aucun fournisseur.</p><button class="btn bpk mt3" onclick="oNewFou()">+ Ajouter le premier fournisseur</button></div>`;return;}
  const total=fous.reduce((s,f)=>s+(f.ac||0),0);
  el.innerHTML=`<div class="kpi mb4" style="border-left:4px solid var(--pk);max-width:280px;"><div class="kl">Total achats (derniers)</div><div class="kv" style="color:var(--pk)">${f(total)}</div><div class="ks">FCFA</div></div>
  <div class="fou-g">${fous.map(fo=>`<div class="fou-c">
    <div class="flex aic jb"><div><div style="font-size:14px;font-weight:800;">${es(fo.nm)}</div><div class="txs tmu mt2">📦 ${es(fo.ct||'')}</div></div>
    <div class="flex gi2"><button class="bico ed" onclick="oEditFou('${fo.id}')">✏️</button><button class="bico dl" onclick="cDelFou('${fo.id}','${es(fo.nm)}')">🗑️</button></div></div>
    ${fo.tl?`<div class="txs tmu mt2">📞 ${es(fo.tl)}</div>`:''}
    <div style="font-size:13px;font-weight:700;color:var(--am);margin-top:6px;">Dernier achat : ${f(fo.ac||0)} FCFA ${fo.dt?'· '+fo.dt:''}</div>
    ${fo.nt?`<div class="txs tmu mt2">📝 ${es(fo.nt)}</div>`:''}
  </div>`).join('')}</div>`;
}
function oNewFou(){eFouId=null;document.getElementById('fou-mot').textContent='🏭 Nouveau fournisseur';sv('fou-nm','');sv('fou-ct','');sv('fou-tl','');sv('fou-ac','0');sv('fou-dt',td());sv('fou-nt','');document.getElementById('bsfou').textContent='Enregistrer';ov('ov-fou');}
function oEditFou(id){const bi=getBi();if(bi<0)return;const b=S.businesses[bi];const fo=(b.fournisseurs||[]).find(x=>x.id===id);if(!fo)return;eFouId=id;document.getElementById('fou-mot').textContent='✏️ Modifier le fournisseur';sv('fou-nm',fo.nm);sv('fou-ct',fo.ct||'');sv('fou-tl',fo.tl||'');sv('fou-ac',fo.ac||0);sv('fou-dt',fo.dt||td());sv('fou-nt',fo.nt||'');document.getElementById('bsfou').textContent='Mettre à jour';ov('ov-fou');}
async function savFou(){
  const nm=g('fou-nm'),ct=g('fou-ct'),tl=g('fou-tl'),ac=parseFloat(g('fou-ac'))||0,dt=g('fou-dt'),nt=g('fou-nt');
  if(!nm){toast('Nom du fournisseur requis','er');return;}
  const bi=getBi();if(bi<0)return;const b=S.businesses[bi];if(!b.fournisseurs)b.fournisseurs=[];
  if(eFouId){const fo=b.fournisseurs.find(x=>x.id===eFouId);if(fo)Object.assign(fo,{nm,ct,tl,ac,dt,nt});}
  else b.fournisseurs.push({id:uid(),nm,ct,tl,ac,dt,nt});
  cov('ov-fou');sch();rpB(curBizPg);toast('Fournisseur enregistré ✓','sg');
}
function cDelFou(id,nm){document.getElementById('ctxt').textContent=`Supprimer "${nm}" ?`;document.getElementById('bconf').onclick=()=>{const bi=getBi();if(bi<0)return;S.businesses[bi].fournisseurs=S.businesses[bi].fournisseurs.filter(f=>f.id!==id);cov('ov-conf');sch();rpB(curBizPg);};ov('ov-conf');}

// ============================================================
// BIZ SALAIRE
// ============================================================
function rBSal(){
  const el=document.getElementById('salc');const bi=getBi();
  if(bi<0){el.innerHTML='<div class="est"><div class="eico">💰</div><p>Sélectionnez un business.</p></div>';return;}
  const b=S.businesses[bi];const ben=bBen(bi,bmi),vers=bVers(bi,bmi);
  const vlist=b.mois[bmi]?.versements||[];
  const hist=vlist.length?vlist.slice().reverse().map(v=>`
    <div class="vi"><div class="vico">💸</div>
    <div style="flex:1;"><div style="font-size:14px;font-weight:700;">${f(v.mt)} FCFA</div>
    <div class="txs tmu">${v.dt||''} · ${es(v.nt||'Salaire dirigeant')}</div>
    <div class="txs mt2"><span class="bge bgg">→ Ajouté aux revenus personnels (${MNS[mi]?.label||MN[mi]||''})</span></div></div>
    <button class="bico dl" onclick="cDelVers('${v.id}')">🗑️</button></div>`).join(''):'<div class="est"><div class="eico">💰</div><p>Aucun versement ce mois.</p></div>';
  el.innerHTML=`
    <div class="sal-bn">
      <div><div class="sl">💰 Salaire dirigeant — ${MNS[bmi]?.label||MN[bmi]||''}</div><div class="sa">${f(vers)} FCFA versés</div><div class="ss">Bénéfice net : ${f(ben)} FCFA · Salaire prévu : ${f(b.salPrev||0)} FCFA/mois</div></div>
      <button class="sal-bt" onclick="oSal()">💸 Se verser son salaire →</button>
    </div>
    <div class="card mb4">
      <div class="flex aic jb mb3"><div style="font-size:14px;font-weight:700;">Paramètre salaire</div><button class="btn bo bsm" onclick="editSalPrev()">✏️ Modifier</button></div>
      <div class="flex gi3 fw">
        <div class="kpi f1" style="border-left:4px solid var(--cy);min-width:120px;"><div class="kl">Salaire prévu/mois</div><div class="kv" style="color:var(--cy)">${f(b.salPrev||0)}</div><div class="ks">FCFA</div></div>
        <div class="kpi f1" style="border-left:4px solid ${ben>=0?'var(--g)':'var(--r)'};min-width:120px;"><div class="kl">Bénéfice net dispo.</div><div class="kv" style="color:${ben>=0?'var(--g)':'var(--r)'}">${f(ben)}</div><div class="ks">FCFA</div></div>
        <div class="kpi f1" style="border-left:4px solid var(--pu);min-width:120px;"><div class="kl">Déjà versé</div><div class="kv" style="color:var(--pu)">${f(vers)}</div><div class="ks">FCFA ce mois</div></div>
      </div>
    </div>
    <div class="card"><div style="font-size:14px;font-weight:700;margin-bottom:12px;">Historique des versements — ${MNS[bmi]?.label||MN[bmi]||''}</div>${hist}</div>`;
}
function oSal(){
  const bi=getBi();if(bi<0){toast('Sélectionnez un business','er');return;}
  const b=S.businesses[bi];const ben=bBen(bi,bmi),vers=bVers(bi,bmi);
  document.getElementById('sal-inf').innerHTML=`<div><strong>💰 ${es(b.nom)}</strong> — ${MNS[bmi]?.label||MN[bmi]||''}</div><div style="font-size:12px;margin-top:4px;">Bénéfice net : <strong style="color:var(--g)">${f(ben)} FCFA</strong> · Déjà versé : <strong>${f(vers)} FCFA</strong></div><div style="font-size:12px;">Salaire prévu : <strong>${f(b.salPrev||0)} FCFA/mois</strong></div>`;
  sv('sal-am',Math.max(0,Math.min(b.salPrev||0,Math.max(0,ben))));
  sv('sal-dt',td());sv('sal-nt','Salaire dirigeant — '+MNS[bmi]?.label||MN[bmi]||'');
  document.getElementById('sal-calc').innerHTML=`Ce montant sera automatiquement ajouté à vos <strong>revenus personnels du mois ${MNS[mi]?.label||MN[mi]||''}</strong> et réparti dans vos 5 plans (Don, Épargne, Investissement, Dettes, Consommation).`;
  ov('ov-sal');
}
function editSalPrev(){const bi=getBi();if(bi<0)return;const b=S.businesses[bi];const nv=prompt('Nouveau salaire prévu par mois (FCFA) :',b.salPrev||0);if(nv===null)return;b.salPrev=parseFloat(nv)||0;sch();rpB(curBizPg);toast('Salaire prévu mis à jour ✓','sg');}
async function savSal(){
  const amt=parseFloat(g('sal-am'))||0,dt=g('sal-dt'),nt=g('sal-nt');
  if(!amt||amt<1){toast('Montant requis','er');return;}
  const bi=getBi();if(bi<0)return;const b=S.businesses[bi];
  if(!b.mois[bmi])b.mois[bmi]=dBM();if(!b.mois[bmi].versements)b.mois[bmi].versements=[];
  b.mois[bmi].versements.push({id:uid(),mt:amt,dt,nt});
  // Alimente automatiquement les revenus personnels
  M().revenus.push({id:uid(),source:'💰 Salaire — '+b.nom,amount:amt,date:dt,note:'Versement dirigeant '+b.nom,isSalBiz:true,bizId:b.id});
  cov('ov-sal');sch();
  toast(f(amt)+' FCFA versés → revenus perso mis à jour ✓','sg');
  rpB(curBizPg);
}
function cDelVers(id){
  document.getElementById('ctxt').textContent='Supprimer ce versement ? (retiré aussi des revenus personnels)';
  document.getElementById('bconf').onclick=()=>{
    const bi=getBi();if(bi<0)return;const b=S.businesses[bi];
    const v=b.mois[bmi].versements.find(x=>x.id===id);
    if(v){b.mois[bmi].versements=b.mois[bmi].versements.filter(x=>x.id!==id);M().revenus=M().revenus.filter(r=>!(r.isSalBiz&&r.bizId===b.id&&r.amount===v.mt&&r.date===v.dt));}
    cov('ov-conf');sch();rpB(curBizPg);
  };ov('ov-conf');
}

// ============================================================
// BIZ SETTINGS
// ============================================================
function rBSet(){
  const el=document.getElementById('bizsett');
  const businesses=S.businesses||[];
  el.innerHTML=`<div style="font-size:14px;color:var(--s5);margin-bottom:18px;">Chaque business a son propre suivi de commandes, charges fixes, fournisseurs et salaire dirigeant. Le salaire versé alimente automatiquement vos finances personnelles.</div>
  <div class="biz-sel-row">
    ${businesses.map(b=>{
      const bi=S.businesses.indexOf(b);
      const ca=MN.reduce((_,__,i)=>_+bCA(bi,i),0);
      return`<div class="biz-card on">
        <div class="flex aic jb"><div><div class="biz-card-nm">${BT[b.type]||'📦'} ${es(b.nom)}</div><div class="biz-card-tp">${es(b.desc||'')}</div></div>
        <button class="bico dl" onclick="cDelBiz('${b.id}','${es(b.nom)}')">🗑️</button></div>
        <div class="txs tmu mt2">Salaire prévu : ${f(b.salPrev||0)} FCFA/mois</div>
        <div class="txs tmu">CA cumulé : ${f(ca)} FCFA</div>
        <button class="btn bcy bsm mt3" onclick="curBizId='${b.id}';updBSel();gpB('biz-db')">Voir le dashboard →</button>
      </div>`;
    }).join('')}
    <div class="biz-card biz-card-add" onclick="ov('ov-nb')" style="min-width:150px;min-height:80px;">+ Nouveau business</div>
  </div>
  ${!businesses.length?`<div class="est"><div class="eico">🏢</div><p style="font-size:15px;font-weight:700;">Aucun business</p><button class="btn bcy mt3" onclick="ov('ov-nb')">Créer mon premier business</button></div>`:''}`;
}
function savNB(){
  const nm=g('nb-nm'),tp=g('nb-tp'),ds=g('nb-ds'),sal=parseFloat(g('nb-sl'))||0;
  if(!nm){toast('Nom du business requis','er');return;}
  if(!S.businesses)S.businesses=[];
  const nb=dBiz(nm,tp,ds,sal);S.businesses.push(nb);curBizId=nb.id;
  updBSel();cov('ov-nb');sch();toast('Business "'+nm+'" créé ✓','sg');gpB('biz-db');
}
function cDelBiz(id,nm){
  document.getElementById('ctxt').textContent=`Supprimer "${nm}" et toutes ses données ?`;
  document.getElementById('bconf').onclick=()=>{S.businesses=S.businesses.filter(b=>b.id!==id);if(curBizId===id)curBizId=S.businesses[0]?.id||null;updBSel();cov('ov-conf');sch();rBSet();toast('Business supprimé','sg');};ov('ov-conf');
}

// ============================================================
// SHARED UTILS
// ============================================================
function f(n){return Math.round(n||0).toLocaleString('fr-FR');}
function es(s){return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');}
function g(id){return document.getElementById(id)?.value||'';}
function sv(id,val){const e=document.getElementById(id);if(e)e.value=val;}
function td(){return new Date().toISOString().slice(0,10);}
function ov(id){document.getElementById(id)?.classList.add('on');}
function cov(id){document.getElementById(id)?.classList.remove('on');}
function bld(b,t,l=true){if(b){b.textContent=t;b.disabled=l;}}
document.querySelectorAll('.ov').forEach(el=>el.addEventListener('click',e=>{if(e.target===el)el.classList.remove('on');}));
let ttmr=null;
function toast(msg,type='sg'){
  const t=document.getElementById('toast');t.innerHTML=`<span>${msg}</span>`;
  t.className=`toast ${type} on`;clearTimeout(ttmr);ttmr=setTimeout(()=>t.classList.remove('on'),3500);
}
function toastUndo(msg,cb){
  const t=document.getElementById('toast');
  t.innerHTML=`<span>${msg}</span><button class="undo" onclick="(${cb.toString()})();document.getElementById('toast').classList.remove('on')">Annuler</button>`;
  t.className='toast on';clearTimeout(ttmr);ttmr=setTimeout(()=>t.classList.remove('on'),5000);
}
function sErr(m){const e=document.getElementById('aerr');e.textContent=m;e.classList.add('on');}
function hErr(){document.getElementById('aerr').classList.remove('on');}
function stab(t){document.getElementById('tl').classList.toggle('on',t==='l');document.getElementById('tr2').classList.toggle('on',t==='r');document.getElementById('fl').style.display=t==='l'?'flex':'none';document.getElementById('fr').style.display=t==='r'?'flex':'none';hErr();}
function fE(c){const m={'auth/email-already-in-use':'Email déjà utilisé','auth/invalid-email':'Email invalide','auth/wrong-password':'Email ou mot de passe incorrect','auth/user-not-found':'Compte introuvable','auth/weak-password':'Mot de passe trop faible','auth/invalid-credential':'Email ou mot de passe incorrect'};return m[c]||'Erreur : '+c;}


// ============================================================
// V3 - DYNAMIC MONTHS
// ============================================================
function populateMnSel(){
  const opts=MNS.map((m,i)=>`<option value="${i}"${i===mi?' selected':''}>${m.label}</option>`).join('');
  ['msel','mselb'].forEach(id=>{const e=document.getElementById(id);if(e)e.innerHTML=opts;});
}
function getCurrentMonthIdx(){
  const now=new Date(),yr=now.getFullYear(),mo=now.getMonth();
  return MNS.findIndex(m=>m.key===yr+'-'+String(mo+1).padStart(2,'0'));
}

// ============================================================
// V3 - GRAPHIQUES (Chart.js)
// ============================================================
let chartRevDep=null,chartPlans=null,chartBizCA=null;
function rCharts(){
  const el=document.getElementById('charts-content');
  const labels=MNS.slice(0,Math.min(12,S.mois.length)).map(m=>m.label);
  const revs=MNS.slice(0,labels.length).map((_,i)=>tRev(i));
  const deps=MNS.slice(0,labels.length).map((_,i)=>PK.reduce((s,k)=>s+pDep(i,k),0));
  const sols=revs.map((r,i)=>r-deps[i]);
  const hasData=revs.some(v=>v>0)||deps.some(v=>v>0);
  el.innerHTML=`
    <div class="card mb4">
      <div style="font-size:14px;font-weight:700;margin-bottom:4px;">Revenus vs Dépenses — évolution mensuelle</div>
      <div class="tsm tmu mb3">Tous les mois</div>
      <div class="chart-wrap"><canvas id="chart-revdep"></canvas></div>
    </div>
    <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:14px;margin-bottom:18px;">
      <div class="card">
        <div style="font-size:14px;font-weight:700;margin-bottom:4px;">Solde mensuel</div>
        <div class="chart-wrap" style="height:160px;"><canvas id="chart-solde"></canvas></div>
      </div>
      <div class="card">
        <div style="font-size:14px;font-weight:700;margin-bottom:4px;">Répartition des plans — ${MNS[mi].label}</div>
        <div class="chart-wrap" style="height:160px;"><canvas id="chart-plans"></canvas></div>
      </div>
    </div>
    ${buildBizChartHTML()}`;
  setTimeout(()=>{
    if(!hasData){return;}
    // Revenus vs Dépenses
    const ctx1=document.getElementById('chart-revdep')?.getContext('2d');
    if(ctx1){
      if(chartRevDep)chartRevDep.destroy();
      chartRevDep=new Chart(ctx1,{type:'bar',data:{labels,datasets:[
        {label:'Revenus',data:revs,backgroundColor:'rgba(22,163,74,.7)',borderRadius:4},
        {label:'Dépenses',data:deps,backgroundColor:'rgba(220,38,38,.7)',borderRadius:4},
      ]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'top'}},scales:{y:{ticks:{callback:v=>f(v)+' F'}}}}});
    }
    // Solde
    const ctx2=document.getElementById('chart-solde')?.getContext('2d');
    if(ctx2){
      new Chart(ctx2,{type:'line',data:{labels,datasets:[{label:'Solde',data:sols,borderColor:'#2563EB',backgroundColor:'rgba(37,99,235,.1)',fill:true,tension:.3,pointRadius:3}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{y:{ticks:{callback:v=>f(v)+' F'}}}}});
    }
    // Plans pie
    const ctx3=document.getElementById('chart-plans')?.getContext('2d');
    if(ctx3){
      const planVals=PK.map(k=>pDep(mi,k));
      const planColors=['#D97706','#16A34A','#2563EB','#DC2626','#7C3AED'];
      new Chart(ctx3,{type:'doughnut',data:{labels:PK.map(k=>PC[k].lb),datasets:[{data:planVals,backgroundColor:planColors,borderWidth:2}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'bottom',labels:{font:{size:10},boxWidth:10}}}}});
    }
    // Biz chart
    if(S.businesses?.length){
      const bi=getBi();if(bi<0)return;
      const caData=MNS.slice(0,Math.min(12,S.businesses[bi].mois.length)).map((_,i)=>bCA(bi,i));
      const benData=MNS.slice(0,caData.length).map((_,i)=>bBen(bi,i));
      const ctx4=document.getElementById('chart-biz')?.getContext('2d');
      if(ctx4){
        new Chart(ctx4,{type:'bar',data:{labels:labels.slice(0,caData.length),datasets:[
          {label:'CA',data:caData,backgroundColor:'rgba(8,145,178,.7)',borderRadius:4},
          {label:'Bénéfice net',data:benData,backgroundColor:'rgba(22,163,74,.7)',borderRadius:4},
        ]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'top'}},scales:{y:{ticks:{callback:v=>f(v)+' F'}}}}});
      }
    }
  },100);
}
function buildBizChartHTML(){
  if(!S.businesses?.length)return'';
  const bi=getBi();if(bi<0)return'';
  return`<div class="card"><div style="font-size:14px;font-weight:700;margin-bottom:4px;">Business : CA vs Bénéfice net</div><div class="tsm tmu mb3">${es(S.businesses[bi].nom)}</div><div class="chart-wrap"><canvas id="chart-biz"></canvas></div></div>`;
}

// ============================================================
// V3 - OBJECTIFS D'ÉPARGNE
// ============================================================
function rObjectifs(){
  const el=document.getElementById('obj-content');
  const objs=S.objectifs||[];
  const totalEp=S.mois.reduce((s,_,i)=>s+pDep(i,'epargne'),0);
  if(!objs.length){
    el.innerHTML=`<div class="ib ib-g mb4"><span>💡</span><span>Définissez vos objectifs d'épargne (Hajj, maison, voiture…) et suivez votre progression grâce à votre épargne cumulée.</span></div>
    <div class="est"><div class="eico">🎯</div><p>Aucun objectif.</p><button class="btn bg mt3" onclick="oNewObj()">+ Créer mon premier objectif</button></div>`;
    return;
  }
  el.innerHTML=`<div class="kpi mb4" style="border-left:4px solid var(--b);max-width:280px;"><div class="kl">Épargne cumulée totale</div><div class="kv" style="color:var(--b)">${f(totalEp)}</div><div class="ks">FCFA sur tous les mois</div></div>
  ${objs.map(obj=>{
    const pct=obj.cible>0?Math.min(100,(obj.epargneActuelle||totalEp)/obj.cible*100):0;
    const reste=Math.max(0,obj.cible-(obj.epargneActuelle||totalEp));
    const moisRestants=obj.mensualite>0?Math.ceil(reste/obj.mensualite):null;
    return`<div class="obj-card">
      <div class="flex aic jb mb3">
        <div><div class="obj-name">${obj.icone||'🎯'} ${es(obj.nom)}</div><div class="txs tmu mt2">Objectif : <strong>${f(obj.cible)} FCFA</strong></div></div>
        <div class="flex gi2"><button class="bico ed" onclick="oEditObj('${obj.id}')">✏️</button><button class="bico dl" onclick="cDelObj('${obj.id}','${es(obj.nom)}')">🗑️</button></div>
      </div>
      <div class="obj-prog-wrap">
        <div class="flex jb txs tmu mb2"><span>${f(obj.epargneActuelle||totalEp)} FCFA épargnés</span><span style="font-weight:700;color:${pct>=100?'var(--g)':'var(--b)'}">${pct.toFixed(0)}%</span></div>
        <div class="obj-prog-bg"><div class="obj-prog-fill" style="width:${pct}%;background:${pct>=100?'var(--g)':'var(--b)'}"></div></div>
      </div>
      <div class="flex gi2 fw mt3 txs tmu">
        <span>Reste : <strong style="color:var(--r)">${f(reste)} FCFA</strong></span>
        ${moisRestants?`<span>· ~${moisRestants} mois à ${f(obj.mensualite)} FCFA/mois</span>`:''}
        ${obj.dateLimit?`<span>· Échéance : ${obj.dateLimit}</span>`:''}
      </div>
      ${pct>=100?'<div class="tsm fb tg mt3" style="text-align:center">🎉 Objectif atteint !</div>':''}
    </div>`;
  }).join('')}`;
}
function oNewObj(){
  eObjId=null;
  const icons=['🕌','🏠','🚗','✈️','💻','📱','🎓','💍','🏪','🌱','💰','🎯'];
  const picked=icons[Math.floor(Math.random()*icons.length)];
  const modal=`<div class="ov on" id="ov-obj"><div class="mo"><h2>🎯 Nouvel objectif d'épargne</h2><div class="mof">
    <div class="fl"><label>Nom de l'objectif</label><input id="obj-nm" placeholder="Ex: Hajj 2027, Achat terrain…"/></div>
    <div class="fl"><label>Icône</label><select id="obj-ic">${icons.map(ic=>`<option value="${ic}"${ic===picked?' selected':''}>${ic} ${ic}</option>`).join('')}</select></div>
    <div class="fl"><label>Montant cible (FCFA)</label><input id="obj-ci" type="number" placeholder="1500000" min="1"/></div>
    <div class="fl"><label>Épargne actuelle dédiée (FCFA)</label><input id="obj-ea" type="number" placeholder="0" value="0"/></div>
    <div class="fl"><label>Mensualité prévue (FCFA)</label><input id="obj-mn" type="number" placeholder="50000" value="0"/></div>
    <div class="fl"><label>Date limite (optionnelle)</label><input id="obj-dl" type="date"/></div>
    <div class="fl"><label>Note</label><input id="obj-nt" placeholder="Contexte, motivation…"/></div>
  </div><div class="moa"><button class="btn bo" onclick="cov('ov-obj')">Annuler</button><button class="btn bg" onclick="savObj()">Enregistrer</button></div></div></div>`;
  document.body.insertAdjacentHTML('beforeend',modal);
}
let eObjId=null;
function oEditObj(id){
  const obj=S.objectifs.find(o=>o.id===id);if(!obj)return;
  eObjId=id;oNewObj();
  setTimeout(()=>{sv('obj-nm',obj.nom);sv('obj-ic',obj.icone||'🎯');sv('obj-ci',obj.cible);sv('obj-ea',obj.epargneActuelle||0);sv('obj-mn',obj.mensualite||0);sv('obj-dl',obj.dateLimit||'');sv('obj-nt',obj.note||'');},50);
}
function savObj(){
  const nm=g('obj-nm'),ic=g('obj-ic'),ci=parseFloat(g('obj-ci'))||0,ea=parseFloat(g('obj-ea'))||0,mn=parseFloat(g('obj-mn'))||0,dl=g('obj-dl'),nt=g('obj-nt');
  if(!nm||!ci){toast('Nom et montant cible requis','er');return;}
  if(!S.objectifs)S.objectifs=[];
  if(eObjId){const o=S.objectifs.find(x=>x.id===eObjId);if(o)Object.assign(o,{nom:nm,icone:ic,cible:ci,epargneActuelle:ea,mensualite:mn,dateLimit:dl,note:nt});}
  else S.objectifs.push({id:uid(),nom:nm,icone:ic,cible:ci,epargneActuelle:ea,mensualite:mn,dateLimit:dl,note:nt});
  cov('ov-obj');document.getElementById('ov-obj')?.remove();sch();rObjectifs();toast('Objectif enregistré ✓','sg');
}
function cDelObj(id,nm){
  document.getElementById('ctxt').textContent=`Supprimer l'objectif "${nm}" ?`;
  document.getElementById('bconf').onclick=()=>{S.objectifs=S.objectifs.filter(o=>o.id!==id);cov('ov-conf');sch();rObjectifs();};ov('ov-conf');
}

// ============================================================
// V3 - TRANSACTIONS RÉCURRENTES
// ============================================================
function rRecurrentes(){
  const el=document.getElementById('rec-content');
  const recs=S.recurrentes||[];
  if(!recs.length){
    el.innerHTML=`<div class="ib ib-b mb4"><span>ℹ️</span><span>Les transactions récurrentes se répètent automatiquement chaque mois (loyer, abonnements, salaires…). Activez-les pour qu'elles s'appliquent au mois sélectionné.</span></div>
    <div class="est"><div class="eico">🔄</div><p>Aucune transaction récurrente.</p><button class="btn bg mt3" onclick="oNewRec()">+ Créer la première</button></div>`;
    return;
  }
  el.innerHTML=`<div class="ib ib-b mb4"><span>💡</span><span>Cliquez sur <strong>Appliquer au mois</strong> pour ajouter automatiquement toutes les récurrences actives au mois sélectionné (${MNS[mi].label}).</span></div>
  <button class="btn bg mb4" onclick="applyRec()">▶ Appliquer toutes les récurrences — ${MNS[mi].label}</button>
  ${recs.map(r=>`<div class="rec-item">
    <div class="rec-ico">${r.type==='revenu'?'💚':'🔴'}</div>
    <div style="flex:1;"><div style="font-size:14px;font-weight:700;">${es(r.desc)}</div>
    <div class="txs tmu">${r.type==='revenu'?'Revenu':'Dépense · '+es(PC[r.plan]?.lb||r.plan||'')} · ${r.freq||'mensuel'}</div></div>
    <div style="font-size:15px;font-weight:800;color:${r.type==='revenu'?'var(--g)':'var(--r)'};">${r.type==='revenu'?'+':'-'}${f(r.amount)} FCFA</div>
    <div class="flex gi2 ml2"><button class="bico ed" onclick="oEditRec('${r.id}')">✏️</button><button class="bico dl" onclick="cDelRec('${r.id}','${es(r.desc)}')">🗑️</button></div>
  </div>`).join('')}`;
}
function oNewRec(){
  const modal=`<div class="ov on" id="ov-rec"><div class="mo"><h2>🔄 Nouvelle transaction récurrente</h2><div class="mof">
    <div class="fl"><label>Type</label><select id="rec-tp" onchange="this.nextElementSibling&&(document.getElementById('rec-pf').style.display=this.value==='revenu'?'none':'block')"><option value="revenu">💚 Revenu</option><option value="depense">🔴 Dépense</option></select></div>
    <div class="fl"><label>Description</label><input id="rec-ds" placeholder="Ex: Loyer, Salaire, Abonnement…"/></div>
    <div class="fl"><label>Montant (FCFA)</label><input id="rec-am" type="number" placeholder="150000" min="1"/></div>
    <div class="fl" id="rec-pf"><label>Plan</label><select id="rec-pl"><option value="conso">🛒 Consommation</option><option value="epargne">💳 Épargne</option><option value="invest">📈 Investissement</option><option value="dette">⚠️ Dettes</option><option value="don">🤲 Don</option></select></div>
    <div class="fl"><label>Fréquence</label><select id="rec-fr"><option value="mensuel">Mensuel</option><option value="trimestriel">Trimestriel</option></select></div>
    <div class="fl"><label>Note</label><input id="rec-nt" placeholder="Optionnel…"/></div>
  </div><div class="moa"><button class="btn bo" onclick="cov('ov-rec');document.getElementById('ov-rec')?.remove()">Annuler</button><button class="btn bg" onclick="savRec()">Enregistrer</button></div></div></div>`;
  document.body.insertAdjacentHTML('beforeend',modal);
}
let eRecId=null;
function oEditRec(id){
  const r=S.recurrentes.find(x=>x.id===id);if(!r)return;eRecId=id;oNewRec();
  setTimeout(()=>{sv('rec-tp',r.type);sv('rec-ds',r.desc);sv('rec-am',r.amount);sv('rec-pl',r.plan||'conso');sv('rec-fr',r.freq||'mensuel');sv('rec-nt',r.note||'');document.getElementById('rec-pf').style.display=r.type==='revenu'?'none':'block';},50);
}
function savRec(){
  const tp=g('rec-tp'),ds=g('rec-ds'),am=parseFloat(g('rec-am'))||0,pl=g('rec-pl'),fr=g('rec-fr'),nt=g('rec-nt');
  if(!ds||!am){toast('Description et montant requis','er');return;}
  if(!S.recurrentes)S.recurrentes=[];
  if(eRecId){const r=S.recurrentes.find(x=>x.id===eRecId);if(r)Object.assign(r,{type:tp,desc:ds,amount:am,plan:pl,freq:fr,note:nt});eRecId=null;}
  else S.recurrentes.push({id:uid(),type:tp,desc:ds,amount:am,plan:tp==='revenu'?null:pl,freq:fr,note:nt,actif:true});
  cov('ov-rec');document.getElementById('ov-rec')?.remove();sch();rRecurrentes();toast('Récurrence enregistrée ✓','sg');
}
function cDelRec(id,ds){
  document.getElementById('ctxt').textContent=`Supprimer la récurrence "${ds}" ?`;
  document.getElementById('bconf').onclick=()=>{S.recurrentes=S.recurrentes.filter(r=>r.id!==id);cov('ov-conf');sch();rRecurrentes();};ov('ov-conf');
}
function applyRec(){
  const recs=S.recurrentes||[];if(!recs.length){toast('Aucune récurrence configurée','er');return;}
  const m=M();let count=0;
  recs.forEach(r=>{
    const already=(r.type==='revenu'?m.revenus:m.depenses).some(x=>x.recId===r.id);
    if(!already){
      if(r.type==='revenu')m.revenus.push({id:uid(),source:r.desc,amount:r.amount,date:td(),note:'Récurrence auto',recId:r.id});
      else m.depenses.push({id:uid(),desc:r.desc,amount:r.amount,date:td(),plan:r.plan||'conso',note:'Récurrence auto',recId:r.id});
      count++;
    }
  });
  sch();rp(cp);toast(count>0?count+' transaction(s) ajoutée(s) ✓':'Déjà appliqué ce mois','sg');
}

// ============================================================
// V3 - ANALYSE IA (Claude API)
// ============================================================
let iaLoading=false;
function rIA(){
  const el=document.getElementById('ia-content');
  const rev=tRev(mi),tdp=PK.reduce((s,k)=>s+pDep(mi,k),0),sol=rev-tdp;
  const epargne=pDep(mi,'epargne'),invest=pDep(mi,'invest');
  const topDep=PK.map(k=>({k,d:pDep(mi,k)})).sort((a,b)=>b.d-a.d).slice(0,3);
  const nbDettes=(M().dettes||[]).filter(d=>d.total-d.paye>0).length;
  const nbBiz=S.businesses?.length||0;
  el.innerHTML=`<div class="ia-box">
    <div style="font-size:12px;font-weight:700;color:#64748B;text-transform:uppercase;letter-spacing:.5px;margin-bottom:4px;">🤖 Assistant IA Xiif XamXam</div>
    <div style="font-size:18px;font-weight:800;margin-bottom:4px;">Analyse personnalisée</div>
    <div style="font-size:13px;color:#94A3B8;">Basée sur tes données réelles de ${MNS[mi].label}</div>
  </div>
  <div class="card mb4">
    <div style="font-size:13px;font-weight:700;margin-bottom:10px;">Ton résumé financier du mois</div>
    <div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:14px;">
      <span class="fact-pill">💚 Revenus : ${f(rev)} FCFA</span>
      <span class="fact-pill">🔴 Dépenses : ${f(tdp)} FCFA</span>
      <span class="fact-pill" style="background:${sol>=0?'var(--gl)':'var(--rl)'};color:${sol>=0?'var(--g)':'var(--r)'};">⚖️ Solde : ${f(sol)} FCFA</span>
      <span class="fact-pill">💳 Épargne : ${f(epargne)} FCFA</span>
      <span class="fact-pill">📈 Investissement : ${f(invest)} FCFA</span>
      ${nbDettes>0?`<span class="fact-pill" style="background:var(--rl);color:var(--r);">⚠️ ${nbDettes} dette(s) en cours</span>`:''}
      ${nbBiz>0?`<span class="fact-pill">🏢 ${nbBiz} business</span>`:''}
    </div>
    <div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:16px;">
      <button class="btn bg bsm" onclick="askIA('analyse')">📊 Analyse globale</button>
      <button class="btn bo bsm" onclick="askIA('conseils')">💡 Conseils pratiques</button>
      <button class="btn bb bsm" onclick="askIA('epargne')">💳 Optimiser mon épargne</button>
      <button class="btn bam bsm" onclick="askIA('dettes')">⚠️ Stratégie dettes</button>
      ${nbBiz>0?`<button class="btn bcy bsm" onclick="askIA('business')">🏢 Analyse business</button>`:''}
    </div>
  </div>
  <div id="ia-resp-zone"></div>`;
}
async function askIA(type){
  if(iaLoading){toast('Analyse en cours…','er');return;}
  iaLoading=true;
  const zone=document.getElementById('ia-resp-zone');
  if(!zone)return;
  zone.innerHTML='<div class="ia-resp" style="color:var(--s4);">🤖 Analyse en cours… quelques secondes.</div>';

  // Build rich context from user data
  const rev=tRev(mi),tdp=PK.reduce((s,k)=>s+pDep(mi,k),0),sol=rev-tdp;
  const dettesActives=(M().dettes||[]).filter(d=>d.total-d.paye>0);
  const totalDettes=dettesActives.reduce((s,d)=>s+(d.total-d.paye),0);
  const planDetails=PK.map(k=>{const b=bgt(mi,k),d=pDep(mi,k);return `${PC[k].lb}: budget ${f(b)} FCFA, dépensé ${f(d)} FCFA (${b>0?(d/b*100).toFixed(0):0}%)`;}).join('; ');
  const bizSummary=S.businesses?.length?S.businesses.map(b=>{const bi=S.businesses.indexOf(b);return `${b.nom}: CA ${f(bCA(bi,bmi))} FCFA, bénéfice ${f(bBen(bi,bmi))} FCFA`;}).join('; '):'Aucun business';
  const objectifsInfo=(S.objectifs||[]).map(o=>`${o.icone||'🎯'} ${o.nom}: cible ${f(o.cible)} FCFA`).join('; ')||'Aucun objectif';

  const prompts={
    analyse:`Tu es un coach financier islamique et expert en gestion de finances personnelles en Afrique de l'Ouest (FCFA). Analyse les finances de cet utilisateur pour ${MNS[mi].label} et donne une analyse claire et honnête.

DONNÉES FINANCIÈRES:
- Revenus du mois: ${f(rev)} FCFA
- Dépenses totales: ${f(tdp)} FCFA  
- Solde: ${f(sol)} FCFA
- Plans: ${planDetails}
- Dettes actives: ${dettesActives.length} dette(s), total dû: ${f(totalDettes)} FCFA
- Business: ${bizSummary}
- Objectifs épargne: ${objectifsInfo}

Donne une analyse structurée avec: 1) Points positifs 2) Points d'attention 3) Un score santé financière /10 avec justification. Sois direct, pratique, et adapté au contexte sénégalais.`,

    conseils:`Tu es un coach financier islamique et expert en gestion de finances personnelles en Afrique de l'Ouest (FCFA). Donne 5 conseils pratiques et actionnables pour améliorer les finances de cet utilisateur.

DONNÉES: Revenus ${f(rev)} FCFA, Dépenses ${f(tdp)} FCFA, Solde ${f(sol)} FCFA. Plans: ${planDetails}. Dettes: ${f(totalDettes)} FCFA. Business: ${bizSummary}.

Les conseils doivent être concrets, réalisables dès ce mois, et adaptés à la réalité économique sénégalaise. Intègre des principes islamiques de gestion (éviter le gaspillage, sadaqa, investissement halal) de façon naturelle.`,

    epargne:`Tu es expert en épargne et investissement islamique en Afrique de l'Ouest. Analyse l'épargne et donne une stratégie personnalisée.

DONNÉES: Épargne ce mois: ${f(pDep(mi,'epargne'))} FCFA (${gp2('epargne')*100}% des revenus). Investissement: ${f(pDep(mi,'invest'))} FCFA. Revenus: ${f(rev)} FCFA. Objectifs: ${objectifsInfo}.

Recommande: comment optimiser l'épargne, quels objectifs prioriser, options d'investissement halal adaptées au Sénégal (BRVM, immobilier, agriculture, commerce).`,

    dettes:`Tu es conseiller en gestion de dettes islamique. Analyse la situation des dettes et propose une stratégie de remboursement.

DETTES ACTIVES: ${dettesActives.map(d=>`${d.nom}: reste ${f(d.total-d.paye)} FCFA${d.dlim?', échéance '+d.dlim:''}`).join('; ')||'Aucune dette active'}.
BUDGET DETTES: ${f(bgt(mi,'dette'))} FCFA/mois. REVENUS: ${f(rev)} FCFA.

Propose une stratégie claire: ordre de remboursement, montants mensuels recommandés, conseils pour éviter de nouveaux endettements. Rappelle l'importance islamique de rembourser ses dettes.`,

    business:`Tu es consultant business spécialisé dans les PME africaines. Analyse les performances business.

BUSINESS: ${S.businesses?.map(b=>{const bi=S.businesses.indexOf(b);return `${b.nom} (${b.type}): CA ${f(bCA(bi,bmi))} FCFA, coûts variables ${f(bCV(bi,bmi))} FCFA, charges fixes ${f(bCF(bi,bmi))} FCFA, bénéfice net ${f(bBen(bi,bmi))} FCFA, salaire dirigeant versé ${f(bVers(bi,bmi))} FCFA`;}).join('; ')||'Aucun business'}.

Analyse: rentabilité, ratio charges/CA, recommandations pour améliorer les marges, conseils de développement business adaptés au marché sénégalais.`
  };

  try{
    const resp=await fetch('https://api.anthropic.com/v1/messages',{
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body:JSON.stringify({
        model:'claude-sonnet-4-20250514',
        max_tokens:1000,
        messages:[{role:'user',content:prompts[type]||prompts.analyse}]
      })
    });
    const data=await resp.json();
    const text=data.content?.map(c=>c.text||'').join('')||'Erreur de réponse.';
    zone.innerHTML=`<div class="card"><div style="font-size:13px;font-weight:700;margin-bottom:10px;color:var(--cy);">🤖 Analyse IA — ${type==='analyse'?'Analyse globale':type==='conseils'?'Conseils pratiques':type==='epargne'?'Stratégie épargne':type==='dettes'?'Stratégie dettes':'Analyse business'}</div><div class="ia-resp">${es(text)}</div></div>`;
  }catch(err){
    zone.innerHTML=`<div class="ib ib-r"><span>❌</span><span>Erreur de connexion à l'IA. Vérifie ta connexion internet.</span></div>`;
  }
  iaLoading=false;
}

// ============================================================
// V3 - FACTURES & DEVIS (Business)
// ============================================================
function rBFactures(){
  const el=document.getElementById('factures-list');
  const bi=getBi();if(bi<0){el.innerHTML='<div class="est"><div class="eico">🧾</div><p>Sélectionnez un business.</p></div>';return;}
  const b=S.businesses[bi];
  if(!b.factures)b.factures=[];
  const facts=b.factures;
  if(!facts.length){
    el.innerHTML=`<div class="ib ib-cy mb4"><span>ℹ️</span><span>Créez des factures et devis pour vos clients. Suivez les paiements et exportez en PDF.</span></div>
    <div class="est"><div class="eico">🧾</div><p>Aucune facture.</p><button class="btn bcy mt3" onclick="oNewFacture()">+ Créer la première facture</button></div>`;return;
  }
  const totalAttente=facts.filter(f=>f.statut==='attente').reduce((s,f)=>s+f.total,0);
  const totalPaye=facts.filter(f=>f.statut==='paye').reduce((s,f)=>s+f.total,0);
  el.innerHTML=`<div class="kg mb4">
    <div class="kpi" style="border-left:4px solid var(--am)"><div class="kl">En attente de paiement</div><div class="kv" style="color:var(--am)">${f(totalAttente)}</div><div class="ks">FCFA</div></div>
    <div class="kpi" style="border-left:4px solid var(--g)"><div class="kl">Total encaissé</div><div class="kv" style="color:var(--g)">${f(totalPaye)}</div><div class="ks">FCFA</div></div>
    <div class="kpi" style="border-left:4px solid var(--b)"><div class="kl">Nombre de factures</div><div class="kv" style="color:var(--b)">${facts.length}</div><div class="ks">total</div></div>
  </div>
  ${facts.slice().reverse().map(fac=>`<div class="inv-card">
    <div class="inv-header">
      <div><div class="inv-num">${es(fac.num||'FAC-'+fac.id.slice(0,6).toUpperCase())}</div><div class="inv-client">${es(fac.client)}</div><div class="txs tmu mt2">${fac.date||''} · ${es(fac.desc||'')}</div></div>
      <div class="flex aic gi2"><span class="inv-status ${fac.statut||'attente'}">${fac.statut==='paye'?'✅ Payé':fac.statut==='envoye'?'📤 Envoyé':'⏳ En attente'}</span></div>
    </div>
    <div class="flex aic jb mt3">
      <div style="font-size:18px;font-weight:800;color:var(--cy);">${f(fac.total)} FCFA</div>
      <div class="flex gi2">
        ${fac.statut!=='paye'?`<button class="btn bg bsm" onclick="marquerPaye('${fac.id}')">✅ Marquer payé</button>`:''}
        <button class="pdf-btn" onclick="exportFacturePDF('${fac.id}')">📄 PDF</button>
        <button class="bico dl" onclick="cDelFact('${fac.id}','${es(fac.client)}')">🗑️</button>
      </div>
    </div>
  </div>`).join('')}`;
}
function oNewFacture(){
  const bi=getBi();if(bi<0){toast('Sélectionnez un business','er');return;}
  const b=S.businesses[bi];
  if(!b.factures)b.factures=[];
  const nextNum='FAC-'+String(b.factures.length+1).padStart(4,'0');
  const modal=`<div class="ov on" id="ov-fact"><div class="mo" style="max-width:500px;"><h2>🧾 Nouvelle facture / Devis</h2><div class="mof">
    <div class="fl"><label>N° Facture</label><input id="fact-num" value="${nextNum}"/></div>
    <div class="fl"><label>Client</label><input id="fact-cl" placeholder="Nom du client…"/></div>
    <div class="fl"><label>Description des services/produits</label><input id="fact-ds" placeholder="Ex: Impression 200 tee-shirts…"/></div>
    <div class="fl"><label>Montant total (FCFA)</label><input id="fact-tt" type="number" placeholder="150000" min="0"/></div>
    <div class="fl"><label>Date</label><input id="fact-dt" type="date" value="${td()}"/></div>
    <div class="fl"><label>Date limite paiement</label><input id="fact-dlp" type="date"/></div>
    <div class="fl"><label>Statut</label><select id="fact-st"><option value="attente">⏳ En attente</option><option value="envoye">📤 Envoyé au client</option><option value="paye">✅ Payé</option></select></div>
    <div class="fl"><label>Note</label><input id="fact-nt" placeholder="Conditions de paiement…"/></div>
  </div><div class="moa"><button class="btn bo" onclick="cov('ov-fact');document.getElementById('ov-fact')?.remove()">Annuler</button><button class="btn bcy" onclick="savFacture()">Enregistrer</button></div></div></div>`;
  document.body.insertAdjacentHTML('beforeend',modal);
}
function savFacture(){
  const num=g('fact-num'),cl=g('fact-cl'),ds=g('fact-ds'),tt=parseFloat(g('fact-tt'))||0,dt=g('fact-dt'),dlp=g('fact-dlp'),st=g('fact-st'),nt=g('fact-nt');
  if(!cl||!tt){toast('Client et montant requis','er');return;}
  const bi=getBi();if(bi<0)return;const b=S.businesses[bi];if(!b.factures)b.factures=[];
  b.factures.push({id:uid(),num,client:cl,desc:ds,total:tt,date:dt,dateLimite:dlp,statut:st,note:nt});
  cov('ov-fact');document.getElementById('ov-fact')?.remove();sch();rBFactures();toast('Facture enregistrée ✓','sg');
}
function marquerPaye(id){
  const bi=getBi();if(bi<0)return;const b=S.businesses[bi];
  const fac=b.factures.find(f=>f.id===id);if(!fac)return;
  fac.statut='paye';fac.datePaiement=td();sch();rBFactures();toast('Facture marquée payée ✓','sg');
}
function cDelFact(id,cl){
  document.getElementById('ctxt').textContent=`Supprimer la facture de "${cl}" ?`;
  document.getElementById('bconf').onclick=()=>{const bi=getBi();if(bi<0)return;S.businesses[bi].factures=S.businesses[bi].factures.filter(f=>f.id!==id);cov('ov-conf');sch();rBFactures();};ov('ov-conf');
}
function exportFacturePDF(id){
  const bi=getBi();if(bi<0)return;const b=S.businesses[bi];
  const fac=b.factures.find(f=>f.id===id);if(!fac)return;
  const html=`<!DOCTYPE html><html><head><meta charset="UTF-8"><title>Facture ${es(fac.num||'')}</title>
  <style>body{font-family:Arial,sans-serif;padding:40px;color:#1F2937;max-width:700px;margin:0 auto;}
  .header{display:flex;justify-content:space-between;margin-bottom:30px;}
  .brand{font-size:22px;font-weight:800;color:#0891B2;}.num{font-size:14px;color:#6B7280;}
  .section{margin-bottom:20px;}.label{font-size:12px;font-weight:700;color:#9CA3AF;text-transform:uppercase;}
  .value{font-size:16px;font-weight:600;margin-top:3px;}
  .total-box{background:#F0F9FF;border:2px solid #0891B2;border-radius:8px;padding:16px;text-align:right;}
  .total-label{font-size:13px;color:#0891B2;font-weight:700;}.total-val{font-size:24px;font-weight:800;color:#0891B2;}
  .status{display:inline-block;padding:4px 12px;border-radius:99px;font-size:12px;font-weight:700;background:${fac.statut==='paye'?'#DCFCE7':'#FEF3C7'};color:${fac.statut==='paye'?'#16A34A':'#D97706'};}
  </style></head><body>
  <div class="header"><div><div class="brand">${es(b.nom)}</div><div style="font-size:13px;color:#6B7280;margin-top:4px;">${es(b.desc||'')}</div></div>
  <div style="text-align:right;"><div class="num">FACTURE ${es(fac.num||'')}</div><div style="font-size:13px;margin-top:4px;">${fac.date||''}</div><span class="status">${fac.statut==='paye'?'✅ Payé':fac.statut==='envoye'?'📤 Envoyé':'⏳ En attente'}</span></div></div>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:24px;">
  <div class="section"><div class="label">Client</div><div class="value">${es(fac.client)}</div></div>
  <div class="section"><div class="label">Date limite paiement</div><div class="value">${fac.dateLimite||'—'}</div></div></div>
  <div class="section"><div class="label">Description</div><div class="value">${es(fac.desc||'')}</div></div>
  ${fac.note?`<div class="section"><div class="label">Note / Conditions</div><div class="value">${es(fac.note)}</div></div>`:''}
  <div class="total-box"><div class="total-label">MONTANT TOTAL</div><div class="total-val">${f(fac.total)} FCFA</div></div>
  <p style="font-size:11px;color:#9CA3AF;margin-top:30px;text-align:center;">Généré par Xiif XamXam Business Manager</p>
  </body></html>`;
  const blob=new Blob([html],{type:'text/html'});
  const url=URL.createObjectURL(blob);
  const a=document.createElement('a');a.href=url;a.download='Facture_'+es(fac.num||fac.id)+'_'+es(fac.client)+'.html';
  a.click();URL.revokeObjectURL(url);
  toast('Facture exportée (ouvrir et imprimer en PDF) ✓','sg');
}

// ============================================================
// V3 - SIDEBAR ALERT BADGES
// ============================================================
function updSBAlerts(){
  // Show red dot on charts nav if any plan >90%
  const hasAlert=PK.some(k=>{const b=bgt(mi,k),d=pDep(mi,k);return b>0&&d/b>=0.9;});
  const alertDot=hasAlert?'<span style="width:7px;height:7px;background:var(--r);border-radius:50%;display:inline-block;margin-left:4px;"></span>':'';
  const dashNav=document.querySelector('.sni[data-p="db"] .lbl');
  if(dashNav)dashNav.innerHTML='Tableau de bord'+alertDot;
}

</script>
</body>
</html>
