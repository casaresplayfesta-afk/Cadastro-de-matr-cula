<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cadastro de Colaboradores</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f0f2f5; margin: 0; padding: 0; }
    .container { max-width: 500px; margin: 60px auto; background: white; padding: 30px; border-radius: 10px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
    h2 { text-align: center; color: #333; margin-bottom: 20px; }
    label { display: block; margin-top: 10px; color: #555; }
    input, select { width: 100%; padding: 10px; margin-top: 5px; border-radius: 5px; border: 1px solid #ccc; font-size: 16px; }
    button { margin-top: 20px; width: 100%; background: #4CAF50; color: white; padding: 12px; border: none; border-radius: 5px; font-size: 18px; cursor: pointer; transition: 0.3s; }
    button:hover { background: #45a049; }
    .success, .error { text-align: center; margin-top: 15px; font-weight: bold; }
    .success { color: green; }
    .error { color: red; }
  </style>
</head>
<body>
  <div class="container">
    <h2>Cadastro de Colaboradores</h2>
    <form id="cadastroForm">
      <label>Nome Completo:</label>
      <input type="text" id="nome" required>

      <label>Matrícula:</label>
      <input type="text" id="matricula" required pattern="\d{4,11}" placeholder="Somente números (4 a 11 dígitos)">

      <label>Cargo:</label>
      <input type="text" id="cargo" required>

      <label>Turno:</label>
      <select id="turno" required>
        <option value="">Selecione</option>
        <option value="Manhã">Manhã</option>
        <option value="Tarde">Tarde</option>
        <option value="Noite">Noite</option>
        <option value="Madrugada">Madrugada</option>
      </select>

      <label>E-mail:</label>
      <input type="email" id="email" required placeholder="exemplo@email.com">

      <button type="submit">Cadastrar</button>
      <p class="success" id="successMsg" style="display:none;">✅ Colaborador cadastrado com sucesso!</p>
      <p class="error" id="errorMsg" style="display:none;">❌ Ocorreu um erro ao cadastrar.</p>
    </form>
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
    import { getFirestore, collection, addDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

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

    document.getElementById("cadastroForm").addEventListener("submit", async (e) => {
      e.preventDefault();
      const nome = document.getElementById("nome").value.trim();
      const matricula = document.getElementById("matricula").value.trim();
      const cargo = document.getElementById("cargo").value.trim();
      const turno = document.getElementById("turno").value;
      const email = document.getElementById("email").value.trim();

      const successMsg = document.getElementById("successMsg");
      const errorMsg = document.getElementById("errorMsg");
      successMsg.style.display = "none";
      errorMsg.style.display = "none";

      try {
        await addDoc(collection(db, "colaboradores"), {
          nome, matricula, cargo, turno, email,
          criadoEm: serverTimestamp()
        });
        successMsg.style.display = "block";
        e.target.reset();
      } catch (err) {
        console.error(err);
        errorMsg.style.display = "block";
      }
    });
  </script>
</body>
</html>
