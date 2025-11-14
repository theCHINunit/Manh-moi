<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Text Password Gate — Google Doc Viewer</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--accent:#10b981;--muted:#94a3b8}
    html,body{height:100%;margin:0;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{
      display:grid;
      place-items:center;
      color:#e6eef8;
      background:url('Tosca Modern Creative Web Development Presentation.png') center/cover no-repeat fixed;
    }
    .card{
      width:360px;
      max-width:95%;
      background:rgba(15,23,36,0.85);
      padding:20px;
      border-radius:14px;
      box-shadow:0 8px 30px rgba(0,0,0,0.6);
    }
    h1{font-size:18px;margin:0 0 8px}
    p{margin:0 0 14px;color:var(--muted);font-size:13px}
    .display{height:48px;background:rgba(255,255,255,0.05);border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:20px;letter-spacing:6px}
    .keypad{display:grid;grid-template-columns:repeat(6,1fr);gap:8px;margin-top:12px}
    button.key{height:48px;border-radius:8px;border:0;background:#071226;color:#dff7ef;font-size:16px;cursor:pointer;box-shadow:inset 0 -2px 0 rgba(0,0,0,0.2)}
    .controls{display:flex;gap:10px;margin-top:10px}
    .controls button{flex:1;height:40px;border-radius:8px;border:0;background:#0b2a3a;color:#dff7ef;cursor:pointer}
    .success{padding:10px;border-radius:8px;background:linear-gradient(90deg,rgba(16,185,129,0.12),rgba(16,185,129,0.04));color:#9ef0c9;font-size:14px;margin-top:10px}
    .small{font-size:12px;color:var(--muted)}
  </style>
</head>
<body>
  <div class="card">
    <h1>Nhập mật khẩu chữ</h1>
    <p>Nhập mật khẩu để mở Google Docs (7 chữ cái).</p>

    <div class="display" id="display">●●●●●●●</div>

    <div class="keypad" id="keypad">
      <button class="key">A</button><button class="key">B</button><button class="key">C</button>
      <button class="key">D</button><button class="key">E</button><button class="key">F</button>
      <button class="key">G</button><button class="key">H</button><button class="key">I</button>
      <button class="key">J</button><button class="key">K</button><button class="key">L</button>
      <button class="key">M</button><button class="key">N</button><button class="key">O</button>
      <button class="key">P</button><button class="key">Q</button><button class="key">R</button>
      <button class="key">S</button><button class="key">T</button><button class="key">U</button>
      <button class="key">V</button><button class="key">W</button><button class="key">X</button>
      <button class="key">Y</button><button class="key">Z</button>
      <button class="key">←</button><button class="key">OK</button>
    </div>

    <div class="controls">
      <button id="clear">Xóa</button>
      <button id="showPlain">Hiện chữ</button>
    </div>

    <div id="resultArea"></div>

  </div>

  <script>
    const PASSWORD = 'MANHMOI';
    const DOC_LINK = 'https://docs.google.com/document/d/18RK02c4Fnp0wuqxFjt8WO6SenajLcWNiW9JniBrAEjs/edit?hl=vi&tab=t.0';

    const display = document.getElementById('display');
    const keys = document.querySelectorAll('.key');
    const clearBtn = document.getElementById('clear');
    const showPlain = document.getElementById('showPlain');
    const resultArea = document.getElementById('resultArea');

    let input = '';
    let showLetters = false;

    function updateDisplay(){
      if(input.length === 0){ display.textContent = '●●●●●●●'; return; }
      display.textContent = showLetters ? input : '•'.repeat(input.length);
    }

    keys.forEach(k=>k.addEventListener('click', ()=>{
      const v = k.textContent.trim();
      if(v === 'OK') return checkPassword();
      if(v === '←'){ input = input.slice(0,-1); } 
      else { if(input.length < 7 && /^[A-Z]$/.test(v)) input += v; }
      updateDisplay();
    }));

    clearBtn.addEventListener('click', ()=>{ input=''; updateDisplay(); resultArea.innerHTML=''; });
    showPlain.addEventListener('click', ()=>{ showLetters = !showLetters; showPlain.textContent = showLetters ? 'Ẩn chữ' : 'Hiện chữ'; updateDisplay(); });

    function checkPassword(){
      if(input === PASSWORD){
        resultArea.innerHTML = '<div class="success">✅ Mật khẩu chính xác — mở Google Docs...</div>';
        window.open(DOC_LINK, '_blank');
      } else {
        resultArea.innerHTML = '<div class="success" style="background:rgba(255,70,70,0.08);color:#ffc6c6">❌ Mật khẩu sai. Thử lại.</div>';
        input = ''; updateDisplay();
      }
    }

    document.addEventListener('keydown', (e)=>{
      if(/^[A-Z]$/i.test(e.key) && input.length < 7) { input += e.key.toUpperCase(); updateDisplay(); }
      if(e.key === 'Backspace'){ input = input.slice(0,-1); updateDisplay(); }
      if(e.key === 'Enter') checkPassword();
    });

    updateDisplay();
  </script>
</body>
</html>
