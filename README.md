*🐍 Snake Quiz - Jogo da Cobra Programadora*

📖 Sobre o Jogo
Snake Quiz é um jogo educativo feito em Python que mistura a clássica mecânica da cobrinha com perguntas de programação. 

 🎯 Para que ele serve?
Este jogo foi criado com 3 objetivos principais:

1. Revisar conteúdo de programação de forma divertida
   Em vez de estudar decorando, o jogador aprende respondendo perguntas sobre HTML, CSS e Python enquanto joga. Cada comida `*` que a cobra come gera uma pergunta.

2. Praticar lógica de programação  
   O código usa conceitos fundamentais: funções, listas, dicionários, laços `while`, condicionais `if/else` e input do usuário. Serve como exemplo prático pra quem tá começando.

3. Treinar raciocínio rápido
   Com cronômetro de 60 segundos, o jogador precisa pensar rápido: mover a cobra E responder a pergunta antes do tempo acabar.

🎮 Como funciona
- Use `W` Cima | `S` Baixo | `A` Esquerda | `D` Direita pra mover a cobra `O`
- Quando a cobra come a comida `*`, aparece uma pergunta de programação
- Acertou = +1 ponto | Errou = -1 ponto  
- Game Over se bater na parede ou se o tempo de 60s acabar

💻 Tecnologias usadas
- Python 
- VS Code
- Bibliotecas: random, time, os
  
👥 Feito por:
Ana Beatriz Gomes da Silva,Ana Beatriz Silva Santos,Ana Beatriz Costa Santos,Bruna Totti Ribeiro e Gustavo Henrique França.

LINK DO JOGO:
file:///C:/Users/LabInfo/Desktop/feiradeamostra/snake-quiz.html











CÓDIGO DO JOGO:


<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🐍 Snake Quiz Pro</title>
<style>
:root {
  --bg: #0a0f1a;
  --card: rgba(255, 255, 255, 0.05);
  --border: rgba(0, 255, 136, 0.3);
  --accent: #00ff88;
  --accent2: #ff0055;
  --text: #e0e0e0;
}

* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  background: radial-gradient(1200px 600px at 80% -10%, #1a2a44, var(--bg));
  color: var(--text);
  font-family: 'Segoe UI', system-ui, Arial, sans-serif;
  display: grid;
  place-items: center;
  min-height: 100vh;
  padding: 20px;
}

.tela {
  background: var(--card);
  backdrop-filter: blur(12px);
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 30px;
  width: 100%;
  max-width: 480px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.4);
  text-align: center;
  animation: fadeIn 0.4s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

h1 {
  color: var(--accent);
  font-size: clamp(28px, 5vw, 36px);
  margin-bottom: 10px;
  text-shadow: 0 0 10px var(--accent);
}

h2 { font-size: 18px; margin-bottom: 15px; color: var(--accent); }

p { line-height: 1.6; opacity: 0.9; }

input, button {
  width: 100%;
  padding: 12px 16px;
  font-size: 16px;
  border-radius: 12px;
  border: 1px solid var(--border);
  background: rgba(0,0,0,0.3);
  color: var(--text);
  outline: none;
  transition: all 0.2s;
}

input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px rgba(0,255,136,0.2); }

button {
  background: linear-gradient(135deg, var(--accent), #00cc70);
  color: #001a0d;
  font-weight: 700;
  cursor: pointer;
  border: none;
  margin-top: 12px;
}
button:hover { transform: translateY(-2px); filter: brightness(1.1); }
button:active { transform: scale(0.98); }

#telaRegras { text-align: left; }
#telaRegras p { margin: 8px 0; }

#jogo { max-width: 520px; }

#hud {
  display: flex;
  justify-content: space-between;
  margin-bottom: 12px;
  font-weight: 700;
  font-size: 14px;
}

canvas {
  background: #000;
  border: 2px solid var(--accent);
  border-radius: 12px;
  width: 100%;
  max-width: 400px;
  aspect-ratio: 1/1;
  box-shadow: 0 0 20px rgba(0,255,136,0.2);
}

#perguntaBox {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 15px;
  margin: 15px 0;
  min-height: 60px;
  display: grid;
  place-items: center;
  font-size: 17px;
  font-weight: 600;
}

#resposta { margin-bottom: 0; }

.escondido { display: none!important; }
.dica { font-size: 13px; opacity: 0.7; margin-top: 8px; }
</style>
</head>
<body>

<!-- TELA 1: NOME -->
<div id="telaInicio" class="tela">
  <h1>🐍 SNAKE QUIZ</h1>
  <p>Teste seus conhecimentos de programação enquanto joga Snake</p>
  <br>
  <input type="text" id="nomeAluno" placeholder="Digite seu nome" maxlength="20">
  <button onclick="mostrarRegras()">Começar</button>
</div>

<!-- TELA 2: REGRAS -->
<div id="telaRegras" class="tela escondido">
  <h1>📋 Como Jogar</h1>
  <p><b>1. Movimento:</b> Use as SETAS do teclado</p>
  <p><b>2. Objetivo:</b> Coma o quadrado vermelho</p>
  <p><b>3. Quiz:</b> Cada comida = 1 pergunta de código</p>
  <p><b>4. Pontos:</b> Acertou +1 | Errou 0</p>
  <p><b>5. Game Over:</b> Bateu na parede ou em si mesmo</p>
  <p><b>6. Dificuldade:</b> A cada 5 pontos o jogo acelera</p>
  <br>
  <p>Jogador: <b id="nomeJogador"></b></p>
  <button onclick="iniciarJogo()">Entendi, Bora Jogar!</button>
</div>

<!-- TELA 3: JOGO -->
<div id="jogo" class="tela escondido">
  <div id="hud">
    <span>👤 <span id="nomeNoJogo"></span></span>
    <span>🏆 Recorde: <span id="recorde">0</span></span>
    <span>⭐ Pontos: <span id="pontos">0</span></span>
  </div>

  <canvas id="canvas" width="400" height="400"></canvas>

  <div id="perguntaBox">
    <span id="perguntaTexto">Use as setas para começar...</span>
  </div>

  <input type="text" id="resposta" placeholder="Digite sua resposta e Enter" class="escondido">
  <button id="btnResponder" onclick="verificarResposta()" class="escondido">Responder</button>
  <p class="dica">Dica: Respostas sem acento, em minúsculo</p>
</div>

<script>
const TAMANHO = 20;
const GRID = 20;
let nome = "";
let pontos = 0;
let recorde = localStorage.getItem('snakeRecorde') || 0;
let velocidade = 150; // ms entre frames

let canvas = document.getElementById('canvas');
let ctx = canvas.getContext('2d');
let cobra = [{x: 10, y: 10}, {x: 9, y: 10}, {x: 8, y: 10}]; // começa com 3 blocos pra ver
let comida = {x: 15, y: 10};
let dx = 1, dy = 0; // começa andando pra direita
let jogoRodando = false;
let perguntaAtiva = false;
let perguntaAtual = null;
let lastTime = 0;

document.getElementById('recorde').innerText = recorde;

const perguntas = [
  {p: "Qual tag cria um parágrafo em HTML?", r: "p"},
  {p: "Como imprime texto em Python?", r: "print"},
  {p: "Símbolo de comentário em Python?", r: "#"},
  {p: "Loop que repete com contador?", r: "for"},
  {p: "Comando pra sair de um loop?", r: "break"},
  {p: "Tag de link em HTML?", r: "a"},
  {p: "Tipo de dado True/False?", r: "boolean"},
  {p: "Função pra pegar elemento por id?", r: "getelementbyid"},
  {p: "Como declara variável em JS moderno?", r: "const"},
  {p: "Operador de igualdade estrita?", r: "==="},
  {p: "Método pra adicionar item no fim do array?", r: "push"},
  {p: "CSS pra deixar texto em negrito?", r: "font-weight"},
  {p: "Como comenta em HTML?", r: "<!-- -->"},
  {p: "Método pra remover último item do array?", r: "pop"}
];

function mostrarRegras() {
  nome = document.getElementById('nomeAluno').value.trim() || "Jogador";
  document.getElementById('nomeJogador').innerText = nome;
  document.getElementById('telaInicio').classList.add('escondido');
  document.getElementById('telaRegras').classList.remove('escondido');
}

function iniciarJogo() {
  document.getElementById('telaRegras').classList.add('escondido');
  document.getElementById('jogo').classList.remove('escondido');
  document.getElementById('nomeNoJogo').innerText = nome;
  novaComida();
  jogoRodando = true;
  requestAnimationFrame(loop);
}

document.addEventListener('keydown', (e) => {
  if(!jogoRodando || perguntaAtiva) {
    if(e.key === 'Enter' && perguntaAtiva) verificarResposta();
    return;
  }
  if(e.key === 'ArrowUp' && dy === 0) {dx = 0; dy = -1;}
  if(e.key === 'ArrowDown' && dy === 0) {dx = 0; dy = 1;}
  if(e.key === 'ArrowLeft' && dx === 0) {dx = -1; dy = 0;}
  if(e.key === 'ArrowRight' && dx === 0) {dx = 1; dy = 0;}
});

function novaComida() {
  let x, y;
  do {
    x = Math.floor(Math.random() * GRID);
    y = Math.floor(Math.random() * GRID);
  } while(cobra.some(p => p.x === x && p.y === y));
  comida = {x, y};
}

function loop(time) {
  if(!jogoRodando) return;

  if(time - lastTime > velocidade) {
    lastTime = time;
    atualizar();
  }
  desenhar();
  requestAnimationFrame(loop);
}

function atualizar() {
  if(perguntaAtiva) return;

  let cabeca = {x: cobra[0].x + dx, y: cobra[0].y + dy};

  // Game Over: parede ou bateu em si mesma
  if(cabeca.x < 0 || cabeca.x >= GRID || cabeca.y < 0 || cabeca.y >= GRID ||
     cobra.some(p => p.x === cabeca.x && p.y === cabeca.y)) {
    return gameOver();
  }

  cobra.unshift(cabeca);

  if(cabeca.x === comida.x && cabeca.y === comida.y) {
    perguntaAtiva = true;
    mostrarPergunta();
    // não dá pop = cobra cresce
  } else {
    cobra.pop();
  }
}

function desenhar() {
  ctx.fillStyle = '#000';
  ctx.fillRect(0, 0, 400, 400);

  // Comida com brilho
  let corAccent2 = getComputedStyle(document.body).getPropertyValue('--accent2');
  ctx.fillStyle = corAccent2;
  ctx.shadowColor = corAccent2;
  ctx.shadowBlur = 15;
  ctx.fillRect(comida.x * TAMANHO, comida.y * TAMANHO, TAMANHO, TAMANHO);
  ctx.shadowBlur = 0;

  // Cobra
  let corAccent = getComputedStyle(document.body).getPropertyValue('--accent');
  ctx.fillStyle = corAccent;
  cobra.forEach((parte, i) => {
    ctx.globalAlpha = i === 0? 1 : 0.8; // cabeça mais forte
    ctx.fillRect(parte.x * TAMANHO, parte.y * TAMANHO, TAMANHO-1, TAMANHO-1);
  });
  ctx.globalAlpha = 1;
}

function mostrarPergunta() {
  perguntaAtual = perguntas[Math.floor(Math.random() * perguntas.length)];
  document.getElementById('perguntaTexto').innerText = "❓ " + perguntaAtual.p;
  document.getElementById('resposta').classList.remove('escondido');
  document.getElementById('btnResponder').classList.remove('escondido');
  document.getElementById('resposta').focus();
}

function verificarResposta() {
  if(!perguntaAtiva) return;
  let resp = document.getElementById('resposta').value.toLowerCase().trim().normalize("NFD").replace(/[\u0300-\u036f]/g, "");

  if(resp === perguntaAtual.r) {
    pontos++;
    document.getElementById('pontos').innerText = pontos;
    if(pontos > recorde) {
      recorde = pontos;
      localStorage.setItem('snakeRecorde', recorde);
      document.getElementById('recorde').innerText = recorde;
    }
    if(pontos % 5 === 0 && velocidade > 80) velocidade -= 10; // acelera a cada 5 pontos
    alert('✅ Acertou! +1 ponto');
  } else {
    alert('❌ Errou! Resposta: ' + perguntaAtual.r);
  }

  document.getElementById('resposta').value = '';
  document.getElementById('resposta').classList.add('escondido');
  document.getElementById('btnResponder').classList.add('escondido');
  document.getElementById('perguntaTexto').innerText = 'Continue jogando!';
  novaComida();
  perguntaAtiva = false;
  dx = 1; dy = 0; // volta a andar pra direita
}

function gameOver() {
  jogoRodando = false;
  alert(`💀 Game Over, ${nome}!\nPontos: ${pontos}\nRecorde: ${recorde}`);
  location.reload();
}
</script>
</body>
</html>
