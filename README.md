<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Cadastro de Colaboradores</title>
<style>
  body{font-family:Arial, Helvetica, sans-serif;background:#f0f2f5;margin:0;padding:20px}
  .container{max-width:520px;margin:60px auto;background:#fff;padding:24px;border-radius:10px;box-shadow:0 6px 20px rgba(0,0,0,.06)}
  h2{text-align:center;margin:0 0 12px}
  label{display:block;margin-top:10px;color:#333}
  input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:15px}
  button{background:#2196F3;color:#fff;border:0;cursor:pointer}
  button:hover{opacity:.95}
  .msg{margin-top:12px;text-align:center;font-weight:600}
  .msg.success{color:green}
  .msg.err{color:#c62828}
</style>
</head>
<body>
  <div class="container">
    <h2>Cadastro de Colaboradores</h2>
    <form id="formCadastro">
      <label for="nome">Nome completo</label>
      <input id="nome" type="text" required>
      <label for="matricula">Matrícula (4–11 dígitos)</label>
      <input id="matricula" type="text" pattern="\d{4,11}" required placeholder="Somente números">
      <label for="cargo">Cargo</label>
      <input id="cargo" type="text" required>
      <label for="turno">Turno</label>
      <select id="turno" required>
        <option value="">Selecione</option>
        <option>Manhã</option><option>Tarde</option><option>Noite</option><option>Madrugada</option>
      </select>
      <label for="email">E-mail (obrigatório)</label>
      <input id="email" type="email" required placeholder="seu@exemplo.com">
      <button type="submit">Cadastrar</button>
      <p id="msg" class="msg" style="display:none"></p>
    </form>
  </div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getFirestore, collection, addDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

/* Seu firebaseConfig (ponto-eletronico-f35f9) */
const firebaseConfig = {
  apiKey: "AIzaSyCpBiFzqOod4K32cWMr5hfx13fw6LGcPVY",
  authDomain: "ponto-eletronico-f35f9.firebaseapp.com",
  projectId: "ponto-eletronico-f35f9",
  storageBucket: "ponto-eletronico-f35f9.firebasestorage.app",
  messagingSenderId: "208638350255",
  appId: "1:208638350255:web:63d016867a67575b5e155a"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

const form = document.getElementById('formCadastro');
const msgEl = document.getElementById('msg');

form.addEventListener('submit', async (e)=>{
  e.preventDefault();
  msgEl.style.display='none';
  const nome = form.nome.value.trim();
  const matricula = form.matricula.value.trim();
  const cargo = form.cargo.value.trim();
  const turno = form.turno.value;
  const email = form.email.value.trim();

  if (!nome || !matricula || !cargo || !turno || !email) {
    showMsg('Preencha todos os campos.', true);
    return;
  }

  try {
    await addDoc(collection(db, 'colaboradores'), {
      nome, matricula, cargo, turno, email, criadoEm: serverTimestamp()
    });
    showMsg('✅ Cadastro realizado com sucesso!', false);
    form.reset();
  } catch (err) {
    console.error(err);
    showMsg('Erro ao cadastrar: ' + (err.message||err), true);
  }
});

function showMsg(text, isError){
  msgEl.textContent = text;
  msgEl.className = isError ? 'msg err' : 'msg success';
  msgEl.style.display = 'block';
  setTimeout(()=> msgEl.style.display='none', 5000);
}
</script>
</body>
</html>
