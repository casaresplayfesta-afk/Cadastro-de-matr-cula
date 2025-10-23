<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cadastro de Colaborador</title>
<style>
body { font-family: Arial; background:#f5f5f5; padding:20px; }
.container { max-width:500px; margin:auto; background:white; padding:20px; border-radius:8px; box-shadow:0 0 10px rgba(0,0,0,0.1); }
input, select, button { width:100%; padding:10px; margin:5px 0; border-radius:4px; border:1px solid #ccc; font-size:16px; }
button { background:#4CAF50; color:white; border:none; cursor:pointer; }
button:hover { background:#45a049; }
h1 { text-align:center; color:#333; }
</style>
</head>
<body>
<div class="container">
<h1>Cadastro de Colaborador</h1>
<form id="cadastroForm">
  <input type="text" id="nome" placeholder="Nome Completo" required>
  <input type="text" id="matricula" placeholder="Matrícula (4 a 11 números)" required pattern="\d{4,11}">
  <input type="text" id="cargo" placeholder="Cargo" required>
  <select id="turno" required>
    <option value="">Selecione o Turno</option>
    <option value="Manhã">Manhã</option>
    <option value="Tarde">Tarde</option>
    <option value="Noite">Noite</option>
    <option value="Madrugada">Madrugada</option>
  </select>
  <button type="submit">Cadastrar</button>
</form>
</div>

<script>
document.getElementById('cadastroForm').addEventListener('submit', e=>{
  e.preventDefault();
  const nome=document.getElementById('nome').value.trim();
  const matricula=document.getElementById('matricula').value.trim();
  const cargo=document.getElementById('cargo').value.trim();
  const turno=document.getElementById('turno').value;

  let colaboradores = JSON.parse(localStorage.getItem('colaboradores')) || [];
  if(colaboradores.some(c=>c.matricula===matricula)){
    alert('Já existe um colaborador com esta matrícula!');
    return;
  }

  let ultimoId = colaboradores.length>0?Math.max(...colaboradores.map(c=>c.id)):0;
  ultimoId++;
  colaboradores.push({id:ultimoId, nome, matricula, cargo, turno, inativo:false});
  localStorage.setItem('colaboradores', JSON.stringify(colaboradores));

  alert('Colaborador cadastrado com sucesso!');
  e.target.reset();
});
</script>
</body>
</html>
