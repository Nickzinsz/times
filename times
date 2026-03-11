<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Sorteio de Times Equilibrado</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #1a1a2e, #16213e, #0f3460);
      min-height: 100vh;
      color: #eee;
      padding: 30px 20px;
    }
    h1 {
      text-align: center;
      font-size: 2rem;
      margin-bottom: 30px;
      color: #e94560;
      text-shadow: 0 0 10px rgba(233,69,96,0.5);
    }
    .card {
      background: rgba(255,255,255,0.05);
      border: 1px solid rgba(255,255,255,0.1);
      border-radius: 12px;
      padding: 24px;
      max-width: 700px;
      margin: 0 auto 24px auto;
    }
    label { display: block; margin-bottom: 6px; font-weight: 600; color: #aaa; }
    input[type="number"], input[type="text"] {
      width: 100%;
      padding: 10px 14px;
      border-radius: 8px;
      border: 1px solid rgba(255,255,255,0.15);
      background: rgba(255,255,255,0.08);
      color: #fff;
      font-size: 1rem;
      outline: none;
      transition: border 0.2s;
    }
    input:focus { border-color: #e94560; }
    .btn {
      display: inline-block;
      padding: 10px 22px;
      border-radius: 8px;
      border: none;
      cursor: pointer;
      font-size: 1rem;
      font-weight: 600;
      transition: transform 0.1s, opacity 0.2s;
    }
    .btn:hover { opacity: 0.85; transform: scale(1.02); }
    .btn-primary { background: #e94560; color: #fff; }
    .btn-success { background: #27ae60; color: #fff; }
    .btn-danger  { background: #c0392b; color: #fff; font-size: 0.8rem; padding: 6px 12px; }
    .btn-secondary { background: #555; color: #fff; }
    .player-row {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 10px;
    }
    .player-row input[type="text"] { flex: 1; }
    .player-row input[type="number"] { width: 80px; flex-shrink: 0; }
    .player-row span { color: #aaa; font-size: 0.85rem; white-space: nowrap; }
    #resultado { max-width: 700px; margin: 0 auto; }
    .time-card {
      border-radius: 12px;
      padding: 20px;
      margin-bottom: 20px;
      border: 1px solid rgba(255,255,255,0.1);
    }
    .time-card h3 {
      font-size: 1.2rem;
      margin-bottom: 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .media-badge {
      font-size: 0.85rem;
      padding: 4px 10px;
      border-radius: 20px;
      background: rgba(0,0,0,0.3);
      color: #fff;
    }
    .time-card ul { list-style: none; padding: 0; }
    .time-card ul li {
      display: flex;
      justify-content: space-between;
      padding: 6px 0;
      border-bottom: 1px solid rgba(255,255,255,0.07);
      font-size: 0.95rem;
    }
    .time-card ul li:last-child { border-bottom: none; }
    .nivel-star { color: #f1c40f; font-weight: 700; }
    .cores { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 16px; }
    .cor-btn {
      width: 28px; height: 28px; border-radius: 50%; border: 2px solid transparent;
      cursor: pointer; transition: transform 0.1s;
    }
    .cor-btn:hover { transform: scale(1.2); }
    .cor-btn.ativo { border-color: #fff; transform: scale(1.2); }
    #msg-erro {
      color: #e74c3c;
      font-size: 0.9rem;
      margin-top: 8px;
      display: none;
    }
    .section-title {
      font-size: 1rem;
      font-weight: 700;
      color: #e94560;
      margin-bottom: 14px;
      text-transform: uppercase;
      letter-spacing: 1px;
    }
    .row-btns { display: flex; gap: 10px; margin-top: 16px; flex-wrap: wrap; }
  </style>
</head>
<body>

<h1>⚽ Sorteio de Times Equilibrado</h1>

<!-- PASSO 1 -->
<div class="card">
  <div class="section-title">1. Configuração</div>
  <label>Número de Times</label>
  <input type="number" id="numTimes" min="2" max="20" value="2" />
  <div class="row-btns">
    <button class="btn btn-primary" onclick="configurar()">Configurar Times</button>
  </div>
</div>

<!-- PASSO 2 -->
<div class="card" id="passo2" style="display:none">
  <div class="section-title">2. Adicionar Jogadores</div>
  <div id="listaJogadores"></div>
  <div class="row-btns">
    <button class="btn btn-secondary" onclick="adicionarJogador()">+ Adicionar Jogador</button>
    <button class="btn btn-success" onclick="sortear()">🎲 Sortear Times</button>
  </div>
  <p id="msg-erro">⚠️ Adicione pelo menos 2 jogadores com nomes preenchidos.</p>
</div>

<!-- RESULTADO -->
<div id="resultado"></div>

<script>
  let jogadores = [];

  function configurar() {
    const n = parseInt(document.getElementById('numTimes').value);
    if (isNaN(n) || n < 2) { alert('Informe pelo menos 2 times.'); return; }
    document.getElementById('passo2').style.display = 'block';
    document.getElementById('listaJogadores').innerHTML = '';
    document.getElementById('resultado').innerHTML = '';
    jogadores = [];
    // Adiciona 2 jogadores por padrão
    adicionarJogador();
    adicionarJogador();
  }

  function adicionarJogador() {
    const div = document.getElementById('listaJogadores');
    const idx = div.children.length + 1;
    const row = document.createElement('div');
    row.className = 'player-row';
    row.innerHTML = `
      <span>#${idx}</span>
      <input type="text" placeholder="Nome do jogador" />
      <input type="number" min="1" max="10" value="5" title="Nível (1-10)" />
      <span>nível</span>
      <button class="btn btn-danger" onclick="this.parentElement.remove(); renumerar()">✕</button>
    `;
    div.appendChild(row);
  }

  function renumerar() {
    const rows = document.querySelectorAll('.player-row');
    rows.forEach((r, i) => { r.querySelector('span').textContent = `#${i+1}`; });
  }

  function sortear() {
    const numTimes = parseInt(document.getElementById('numTimes').value);
    const rows = document.querySelectorAll('.player-row');
    const erro = document.getElementById('msg-erro');

    let lista = [];
    rows.forEach(r => {
      const nome = r.querySelectorAll('input')[0].value.trim();
      const nivel = parseInt(r.querySelectorAll('input')[1].value) || 5;
      if (nome) lista.push({ nome, nivel: Math.min(10, Math.max(1, nivel)) });
    });

    if (lista.length < 2) { erro.style.display = 'block'; return; }
    erro.style.display = 'none';

    // Embaralha aleatoriamente
    lista = embaralhar(lista);

    // Ordena por nível decrescente para distribuição equilibrada (snake draft)
    lista.sort((a, b) => b.nivel - a.nivel);

    // Inicializa times
    let times = Array.from({ length: numTimes }, (_, i) => ({ id: i + 1, jogadores: [], soma: 0 }));

    // Distribuição snake: alterna direção a cada rodada para equilibrar
    let direcao = 1;
    let idx = 0;
    lista.forEach((jogador, i) => {
      // Sempre coloca no time com menor soma (greedy equilibrado)
      times.sort((a, b) => a.soma - b.soma);
      times[0].jogadores.push(jogador);
      times[0].soma += jogador.nivel;
    });

    // Embaralha a ordem dos times para não ter "time 1 sempre melhor"
    times = embaralhar(times);
    times.forEach((t, i) => t.id = i + 1);

    renderizarTimes(times);
  }

  function embaralhar(arr) {
    const a = [...arr];
    for (let i = a.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [a[i], a[j]] = [a[j], a[i]];
    }
    return a;
  }

  const CORES = [
    '#e74c3c','#e67e22','#f1c40f','#2ecc71','#1abc9c',
    '#3498db','#9b59b6','#e91e63','#00bcd4','#ff5722',
    '#8bc34a','#607d8b','#ff9800','#673ab7','#009688',
    '#f44336','#2196f3','#4caf50','#ff4081','#795548'
  ];

  function renderizarTimes(times) {
    const div = document.getElementById('resultado');
    div.innerHTML = '<div class="section-title" style="max-width:700px;margin:0 auto 16px auto;">🏆 Times Sorteados</div>';

    times.forEach((time, i) => {
      const cor = CORES[i % CORES.length];
      const media = time.jogadores.length ? (time.soma / time.jogadores.length).toFixed(2) : '0.00';
      const card = document.createElement('div');
      card.className = 'time-card';
      card.style.background = `linear-gradient(135deg, ${cor}22, ${cor}11)`;
      card.style.borderColor = cor + '55';

      let jogadoresHTML = time.jogadores.map(j => `
        <li>
          <span>${j.nome}</span>
          <span class="nivel-star">★ ${j.nivel}</span>
        </li>
      `).join('');

      card.innerHTML = `
        <h3 style="color:${cor}">
          Time ${time.id}
          <span class="media-badge">Média: ${media}</span>
        </h3>
        <ul>${jogadoresHTML}</ul>
      `;
      div.appendChild(card);
    });

    // Botão de novo sorteio
    const btn = document.createElement('div');
    btn.style.textAlign = 'center';
    btn.style.marginTop = '10px';
    btn.innerHTML = `<button class="btn btn-primary" onclick="sortear()">🔄 Novo Sorteio</button>`;
    div.appendChild(btn);

    div.scrollIntoView({ behavior: 'smooth' });
  }
</script>
</body>
</html>
