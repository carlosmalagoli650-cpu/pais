
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Desafio dos Países 🌎</title>
<style>
body {
  margin: 0;
  padding: 20px;
  background: linear-gradient(135deg,#102b50,#176b68);
  color: white;
  font-family: Arial,sans-serif;
  text-align: center;
}
main {
  max-width: 600px;
  margin: auto;
  background: #ffffff16;
  padding: 20px;
  border-radius: 20px;
}
h1 { color: #ffe66d; }
#bandeira {
  font-size: 110px;
  margin: 20px;
}
button {
  display: block;
  width: 100%;
  padding: 14px;
  margin: 9px 0;
  border: 0;
  border-radius: 10px;
  font-size: 17px;
  cursor: pointer;
  background: #fff;
  color: #17334d;
  font-weight: bold;
}
button:hover { background: #ffe66d; }
button:disabled { cursor: default; opacity: .8; }
#mensagem { font-size: 20px; font-weight: bold; }
</style>
</head>
<body>
<main>
<h1>🌎 Desafio dos Países</h1>
<p>Descubra o país pela bandeira!</p>
<p id="nivel">Nível fácil</p>
<p>Pergunta <span id="numero">1</span>/10</p>
<div id="bandeira">🇧🇷</div>
<div id="pergunta">De qual país é esta bandeira?</div>
<div id="opcoes"></div>
<p id="mensagem"></p>
<p>⭐ Pontuação: <span id="pontos">0</span></p>
<button id="proximo" onclick="proxima()" style="display:none">
Próxima pergunta ➡️
</button>
</main>

<script>
const perguntas = [
{b:"🇧🇷",p:"Brasil",o:["Brasil","Portugal","Argentina","México"]},
{b:"🇯🇵",p:"Japão",o:["China","Coreia do Sul","Japão","Tailândia"]},
{b:"🇫🇷",p:"França",o:["Itália","França","Rússia","Holanda"]},
{b:"🇺🇸",p:"Estados Unidos",o:["Canadá","Reino Unido","Austrália","Estados Unidos"]},
{b:"🇮🇹",p:"Itália",o:["Irlanda","Itália","Hungria","Bulgária"]},

{b:"🇰🇬",p:"Quirguistão",o:["Quirguistão","Cazaquistão","Tajiquistão","Uzbequistão"]},
{b:"🇻🇺",p:"Vanuatu",o:["Fiji","Samoa","Vanuatu","Tonga"]},
{b:"🇰🇲",p:"Comores",o:["Maldivas","Comores","Maurício","Seicheles"]},
{b:"🇸🇹",p:"São Tomé e Príncipe",o:["Cabo Verde","Guiné-Bissau","São Tomé e Príncipe","Timor-Leste"]},
{b:"🇧🇹",p:"Butão",o:["Nepal","Mongólia","Butão","Laos"]}
];

let atual = 0;
let pontos = 0;
let respondeu = false;

function mostrar() {
  respondeu = false;
  const q = perguntas[atual];
  document.getElementById("bandeira").textContent = q.b;
  document.getElementById("numero").textContent = atual+1;
  document.getElementById("nivel").textContent =
    atual < 5 ? "🟢 Nível fácil" : "🔥 Nível impossível";
  document.getElementById("mensagem").textContent = "";
  document.getElementById("proximo").style.display = "none";

  const area = document.getElementById("opcoes");
  area.innerHTML = "";
  [...q.o].sort(() => Math.random()-.5).forEach(opcao => {
    const btn = document.createElement("button");
    btn.textContent = opcao;
    btn.onclick = () => responder(opcao,btn);
    area.appendChild(btn);
  });
}

function responder(opcao,btn) {
  if (respondeu) return;
  respondeu = true;
  const q = perguntas[atual];
  const botoes = document.querySelectorAll("#opcoes button");
  botoes.forEach(b => {
    b.disabled = true;
    if (b.textContent === q.p) b.style.background = "#78ef91";
  });

  if (opcao === q.p) {
    pontos += atual < 5 ? 1 : 3;
    document.getElementById("mensagem").textContent =
      "✅ Acertou! +" + (atual < 5 ? 1 : 3) + " pontos!";
  } else {
    document.getElementById("mensagem").textContent =
      "❌ Errou! A resposta era " + q.p + ".";
    btn.style.background = "#ff8a8a";
  }

  document.getElementById("pontos").textContent = pontos;
  document.getElementById("proximo").style.display = "block";
  document.getElementById("proximo").textContent =
    atual === 9 ? "Ver resultado 🏆" : "Próxima pergunta ➡️";
}

function proxima() {
  atual++;
  if (atual >= perguntas.length) {
    document.querySelector("main").innerHTML = `
      <h1>🏆 Desafio concluído!</h1>
      <p style="font-size:65px">🌎</p>
      <h2>Você fez ${pontos} pontos de 20!</h2>
      <p>${pontos === 20 ? "Incrível! Você é mestre dos países!" :
      pontos >= 10 ? "Muito bem! Continue treinando!" :
      "Tente novamente para aprender mais bandeiras!"}</p>
      <button onclick="location.reload()">Jogar novamente 🔄</button>`;
    return;
  }
  mostrar();
}

mostrar();
</script>
</body>
</html>

