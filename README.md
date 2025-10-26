<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Cadastro de Monitor</title>
<style>
  body{font-family:Arial,sans-serif;background:#f0f0f0;margin:0;padding:20px;}
  .card{max-width:500px;margin:50px auto;background:#fff;padding:20px;border-radius:10px;box-shadow:0 0 15px rgba(0,0,0,.1);}
  h1{text-align:center;margin-bottom:15px;}
  input,select,button{width:100%;padding:10px;margin:8px 0;border-radius:6px;border:1px solid #ccc;font-size:16px;}
  button{background:#28a745;color:white;border:none;cursor:pointer;}
  button:hover{background:#218838;}
  p#msg{text-align:center;margin-top:10px;}
</style>
</head>
<body>
  <div class="card">
    <h1>Cadastro de Monitor</h1>
    <form id="formCadastro">
      <input id="nome" placeholder="Nome completo" required />
      <input id="email" type="email" placeholder="Email (opcional)" />
      <input id="matricula" placeholder="Matrícula" required />
      <input id="cargo" placeholder="Cargo" required />
      <select id="turno" required>
        <option value="">Selecione o turno</option>
        <option>Manhã</option>
        <option>Tarde</option>
        <option>Noite</option>
      </select>
      <button type="submit">Cadastrar</button>
      <p id="msg"></p>
    </form>
  </div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getFirestore, collection, addDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

// Configuração Firebase (seu projeto)
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

const form = document.getElementById("formCadastro");
const msg = document.getElementById("msg");

form.addEventListener("submit", async (e) => {
  e.preventDefault();
  const nome = form.nome.value.trim();
  const email = form.email.value.trim();
  const matricula = form.matricula.value.trim();
  const cargo = form.cargo.value.trim();
  const turno = form.turno.value;

  if (!nome || !matricula) {
    msg.textContent = "Nome e matrícula são obrigatórios.";
    msg.style.color = "red";
    return;
  }

  try {
    await addDoc(collection(db, "monitores"), {
      nome, email, matricula, cargo, turno,
      criadoEm: serverTimestamp()
    });
    msg.textContent = "Cadastro realizado com sucesso! ✅";
    msg.style.color = "green";
    form.reset();
  } catch (err) {
    msg.textContent = "Erro: " + err.message;
    msg.style.color = "red";
  }
});
</script>
</body>
</html>
