<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Portal de Reservas · CEEMTI BG</title>
<style>
:root{--bg:#f3f5f8;--card:#fff;--ink:#16202c;--mut:#5b6877;--line:#d6dde5;--pri:#1d4ed8;--pri-ink:#fff;--ok:#12663d;--okbg:#e2f3ea;--busy:#9b2c0c;--busybg:#fbe8e1;--part:#7d5200;--partbg:#fcefcf}
@media(prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#10151c;--card:#19212b;--ink:#e8edf3;--mut:#9aa7b6;--line:#2b3643;--pri:#7aa0ff;--pri-ink:#0b1220;--ok:#74d6a0;--okbg:#17352a;--busy:#ff9f7e;--busybg:#3a2119;--part:#f2c35f;--partbg:#3a2f12}}
:root[data-theme="dark"]{--bg:#10151c;--card:#19212b;--ink:#e8edf3;--mut:#9aa7b6;--line:#2b3643;--pri:#7aa0ff;--pri-ink:#0b1220;--ok:#74d6a0;--okbg:#17352a;--busy:#ff9f7e;--busybg:#3a2119;--part:#f2c35f;--partbg:#3a2f12}
*{box-sizing:border-box}
:root{padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.5 "Segoe UI",system-ui,-apple-system,Roboto,sans-serif}
button,input,select{font:inherit;color:inherit}
:focus-visible{outline:3px solid var(--pri);outline-offset:2px}
header{background:var(--card);border-bottom:1px solid var(--line)}
.bar{max-width:880px;margin:0 auto;padding:12px 16px;display:flex;align-items:center;gap:12px;flex-wrap:wrap}
.brand{font-weight:700;font-size:1.15rem;margin-right:auto}
.brand small{display:block;font-weight:400;color:var(--mut);font-size:.8rem}
main{max-width:880px;margin:0 auto;padding:20px 16px 60px}
h1{font-size:1.5rem;margin:0 0 4px}
h2{font-size:1.15rem;margin:0 0 12px}
p.sub{color:var(--mut);margin:0 0 20px}
.card{background:var(--card);border:1px solid var(--line);border-radius:10px;padding:18px}
nav{display:flex;gap:4px;margin-bottom:18px;border-bottom:1px solid var(--line);overflow-x:auto}
nav button{background:none;border:0;border-bottom:3px solid transparent;padding:10px 14px;cursor:pointer;color:var(--mut);white-space:nowrap}
nav button[aria-current=page]{color:var(--ink);border-color:var(--pri);font-weight:600}
label{display:block;font-weight:600;font-size:.92rem;margin-bottom:4px}
input,select{width:100%;padding:10px 12px;border:1px solid var(--line);border-radius:8px;background:var(--bg)}
.row{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-bottom:18px}
.field{margin-bottom:14px}
button.pri,button.sec,button.dan{border-radius:8px;padding:9px 16px;cursor:pointer;border:1px solid transparent;font-weight:600}
button.pri{background:var(--pri);color:var(--pri-ink)}
button.sec{background:transparent;border-color:var(--line)}
button.dan{background:transparent;border-color:var(--busy);color:var(--busy);padding:6px 12px}
.slots{list-style:none;margin:0;padding:0;display:grid;gap:8px}
.slot{display:grid;grid-template-columns:90px 1fr auto;gap:12px;align-items:center;padding:10px 14px;border-radius:8px;border-left:6px solid}
.slot.ok{background:var(--okbg);border-color:var(--ok)}
.slot.part{background:var(--partbg);border-color:var(--part)}
.slot.busy{background:var(--busybg);border-color:var(--busy)}
.slot span{font-size:.95rem}
.item{display:flex;gap:12px;align-items:center;justify-content:space-between;padding:12px 0;border-bottom:1px solid var(--line)}
.item:last-child{border-bottom:0}
.item small{color:var(--mut);display:block}
.err{color:var(--busy);background:var(--busybg);padding:8px 12px;border-radius:8px;margin-bottom:14px}
.vazio{color:var(--mut);padding:16px 0}
.login{max-width:420px;margin:30px auto}
.tabs{display:flex;gap:8px;margin-bottom:16px}
.tabs button{flex:1}
dialog{border:1px solid var(--line);border-radius:12px;background:var(--card);color:var(--ink);max-width:420px;width:calc(100% - 32px);padding:20px}
dialog::backdrop{background:rgba(0,0,0,.5)}
.acts{display:flex;gap:10px;justify-content:flex-end;margin-top:16px}
#aviso{position:fixed;left:50%;bottom:calc(20px + env(safe-area-inset-bottom,0px));transform:translateX(-50%);background:var(--ink);color:var(--bg);padding:10px 18px;border-radius:8px;display:none}
.tabela{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:.92rem}
th,td{text-align:left;padding:8px 10px;border-bottom:1px solid var(--line);white-space:nowrap}
@media(max-width:560px){.row{grid-template-columns:1fr}.slot{grid-template-columns:1fr auto}.slot strong{grid-column:1/-1}}
</style>
</head>
<body>
<div id="app"></div>
<dialog id="dlg"></dialog>
<div id="aviso" role="status" aria-live="polite"></div>

<script>
/* ===== CONFIGURAÇÃO: edite as listas abaixo para a realidade da escola ===== */
const RECURSOS = [
  { id: 'chrome', nome: 'Chromebooks', grupo: 'Chromebooks', cap: 30 }, // cap = quantidade de unidades
  { id: 'lied1', nome: 'LIED 1 (Laboratório de Informática 1)', grupo: 'LIEDs', cap: 1 },
  { id: 'lied2', nome: 'LIED 2 (Laboratório de Informática 2)', grupo: 'LIEDs', cap: 1 },
  { id: 'lab1', nome: 'Laboratório 1', grupo: 'Laboratórios', cap: 1 },
  { id: 'lab2', nome: 'Laboratório 2', grupo: 'Laboratórios', cap: 1 },
  { id: 'sala1', nome: 'Sala 1', grupo: 'Salas', cap: 1 },
  { id: 'sala2', nome: 'Sala 2', grupo: 'Salas', cap: 1 },
  { id: 'sala3', nome: 'Sala 3', grupo: 'Salas', cap: 1 }
];
const AULAS = ['1ª aula', '2ª aula', '3ª aula', '4ª aula', '5ª aula', '6ª aula', '7ª aula', '8ª aula', '9ª aula'];
const DOMINIO = '@educador.es.gov.br'; // só e-mails deste domínio podem se cadastrar
/* Conta de coordenação criada automaticamente: admin@ceemti.local / admin123 (troque no código) */

/* ===== DADOS (guardados no navegador; veja a nota sobre servidor no fim) ===== */
const K = { u: 'ceemti_usuarios', r: 'ceemti_reservas', s: 'ceemti_sessao' };
const ler = (k, d) => { try { const v = localStorage.getItem(k); return v ? JSON.parse(v) : d; } catch (e) { return d; } };
const gravar = (k, v) => { try { localStorage.setItem(k, JSON.stringify(v)); } catch (e) { aviso('Não foi possível salvar neste navegador.'); } };
let usuarios = ler(K.u, []), reservas = ler(K.r, []), sessaoId = ler(K.s, null);
if (!usuarios.some(u => u.id === 'admin')) {
  usuarios.push({ id: 'admin', nome: 'Coordenação', email: 'admin@ceemti.local', senha: 'admin123', admin: true });
  gravar(K.u, usuarios);
}

/* ===== UTILIDADES ===== */
const esc = s => String(s).replace(/[&<>"']/g, c => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c]));
const hoje = () => new Date(Date.now() - new Date().getTimezoneOffset() * 60000).toISOString().slice(0, 10);
const fmtData = d => d.split('-').reverse().join('/');
const recurso = id => RECURSOS.find(r => r.id === id);
const usuario = () => usuarios.find(u => u.id === sessaoId);
function aviso(t) {
  const el = document.getElementById('aviso');
  el.textContent = t; el.style.display = 'block';
  clearTimeout(aviso.t); aviso.t = setTimeout(() => el.style.display = 'none', 2800);
}

const est = { modo: 'entrar', erro: '', aba: 'reservar', rec: RECURSOS[0].id, data: hoje() };

/* ===== ACESSO ===== */
function entrar(e) {
  e.preventDefault();
  const email = document.getElementById('email').value.trim().toLowerCase();
  const senha = document.getElementById('senha').value;
  const u = usuarios.find(x => x.email === email && x.senha === senha);
  if (!u) { est.erro = 'E-mail ou senha incorretos. Confira os dados e tente de novo.'; return render(); }
  sessaoId = u.id; gravar(K.s, sessaoId); est.erro = ''; est.aba = 'reservar'; render();
}
function cadastrar(e) {
  e.preventDefault();
  const nome = document.getElementById('nome').value.trim();
  const email = document.getElementById('email').value.trim().toLowerCase();
  const senha = document.getElementById('senha').value;
  if (nome.length < 3) { est.erro = 'Digite seu nome completo.'; return render(); }
  if (!email.endsWith(DOMINIO) || email.length <= DOMINIO.length) { est.erro = 'Use seu e-mail institucional, que termina com ' + DOMINIO + '.'; return render(); }
  if (senha.length < 6) { est.erro = 'A senha precisa ter pelo menos 6 caracteres.'; return render(); }
  if (usuarios.some(u => u.email === email)) { est.erro = 'Já existe um cadastro com este e-mail. Use a aba Entrar.'; return render(); }
  const u = { id: 'u' + Date.now(), nome, email, senha, admin: false };
  usuarios.push(u); gravar(K.u, usuarios);
  sessaoId = u.id; gravar(K.s, sessaoId); est.erro = ''; render(); aviso('Cadastro feito. Bem-vindo(a), ' + nome.split(' ')[0] + '!');
}
function sair() { sessaoId = null; gravar(K.s, null); render(); }
function modo(m) { est.modo = m; est.erro = ''; render(); }

/* ===== RESERVAS ===== */
function doSlot(rec, data, aula) { return reservas.filter(r => r.rec === rec && r.data === data && r.aula === aula); }
function abrir(aula) {
  const rec = recurso(est.rec);
  const rs = doSlot(rec.id, est.data, aula);
  const livre = rec.cap - rs.reduce((s, r) => s + r.qtd, 0);
  const d = document.getElementById('dlg');
  d.innerHTML = `<form onsubmit="confirmar(event,${aula})">
    <h2>Reservar ${esc(rec.nome)}</h2>
    <p class="sub">${fmtData(est.data)} · ${AULAS[aula]}</p>
    <div class="field"><label for="turma">Turma</label><input id="turma" required maxlength="30" placeholder="Ex.: 2ª série A"></div>
    ${rec.cap > 1 ? `<div class="field"><label for="qtd">Quantidade de unidades (disponíveis: ${livre})</label><input id="qtd" type="number" min="1" max="${livre}" value="1" required></div>` : ''}
    <div class="acts"><button type="button" class="sec" onclick="document.getElementById('dlg').close()">Cancelar</button><button class="pri">Confirmar reserva</button></div>
  </form>`;
  d.showModal();
  document.getElementById('turma').focus();
}
function confirmar(e, aula) {
  e.preventDefault();
  const rec = recurso(est.rec), u = usuario();
  const qtd = rec.cap > 1 ? parseInt(document.getElementById('qtd').value, 10) : 1;
  const livre = rec.cap - doSlot(rec.id, est.data, aula).reduce((s, r) => s + r.qtd, 0);
  if (est.data < hoje()) return aviso('Não é possível reservar em data passada.');
  if (!(qtd >= 1) || qtd > livre) { document.getElementById('dlg').close(); render(); return aviso('Este horário já foi reservado por outra pessoa. Escolha outro.'); }
  reservas.push({ id: 'r' + Date.now(), rec: rec.id, data: est.data, aula, uid:
