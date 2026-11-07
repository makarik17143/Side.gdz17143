<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Математический тренажёр</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: system-ui, -apple-system, sans-serif;
    background: linear-gradient(135deg, #667eea, #764ba2);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
  }
  .card {
    background: #fff;
    border-radius: 20px;
    padding: 36px;
    width: 100%;
    max-width: 440px;
    box-shadow: 0 20px 50px rgba(0,0,0,.25);
  }
  h1 { font-size: 22px; text-align: center; color: #333; margin-bottom: 6px; }
  .sub { text-align: center; color: #888; font-size: 13px; margin-bottom: 24px; }
  .stats { display: flex; gap: 10px; margin-bottom: 20px; }
  .stat {
    flex: 1; background: #f4f5f9; border-radius: 12px;
    padding: 10px; text-align: center;
  }
  .stat .num { font-size: 20px; font-weight: 700; color: #5561d4; }
  .stat .lbl { font-size: 11px; color: #888; }
  .task {
    font-size: 38px; font-weight: 700; text-align: center;
    color: #333; margin: 24px 0; letter-spacing: 1px;
  }
  input {
    width: 100%; font-size: 24px; padding: 14px;
    text-align: center; border: 2px solid #ddd;
    border-radius: 12px; outline: none; transition: .2s;
  }
  input:focus { border-color: #667eea; }
  input.ok { border-color: #2ecc71; background: #f0fff5; }
  input.err { border-color: #e74c3c; background: #fff5f5; }
  .btns { display: flex; gap: 10px; margin-top: 16px; }
  button {
    flex: 1; padding: 14px; font-size: 16px; font-weight: 600;
    border: none; border-radius: 12px; cursor: pointer; transition: .2s;
  }
  .check { background: #667eea; color: #fff; }
  .check:hover { background: #5561d4; }
  .reset { background: #eee; color: #555; }
  .reset:hover { background: #e0e0e0; }
  .msg { text-align: center; margin-top: 16px; font-weight: 600; min-height: 22px; }
  .msg.ok { color: #2ecc71; }
  .msg.err { color: #e74c3c; }
</style>
</head>
<body>
<div class="card">
  <h1>🧮 Математический тренажёр</h1>
  <p class="sub">Реши пример и нажми «Проверить»</p>

  <div class="stats">
    <div class="stat"><div class="num" id="score">0</div><div class="lbl">Правильно</div></div>
    <div class="stat"><div class="num" id="total">0</div><div class="lbl">Всего</div></div>
    <div class="stat"><div class="num" id="streak">0</div><div class="lbl">Серия</div></div>
  </div>

  <div class="task" id="task">—</div>

  <input type="number" id="answer" placeholder="Ответ" autocomplete="off">

  <div class="btns">
    <button class="check" id="checkBtn">Проверить</button>
    <button class="reset" id="resetBtn">Сброс</button>
  </div>

  <div class="msg" id="msg"></div>
</div>

<script>
  let a, b, op, correct, score = 0, total = 0, streak = 0, answered = false;

  const taskEl = document.getElementById('task');
  const inputEl = document.getElementById('answer');
  const msgEl = document.getElementById('msg');

  function randomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
  }

  function newTask() {
    answered = false;
    inputEl.value = '';
    inputEl.className = '';
    msgEl.textContent = '';
    inputEl.focus();

    op = ['+', '-', '×'][randomInt(0, 2)];
    if (op === '+') { a = randomInt(1, 50); b = randomInt(1, 50); correct = a + b; }
    if (op === '-') { a = randomInt(10, 100); b = randomInt(1, a); correct = a - b; }
    if (op === '×') { a = randomInt(2, 12); b = randomInt(2, 12); correct = a * b; }

    taskEl.textContent = `${a} ${op} ${b} = ?`;
  }

  function updateStats() {
    document.getElementById('score').textContent = score;
    document.getElementById('total').textContent = total;
    document.getElementById('streak').textContent = streak;
  }

  document.getElementById('checkBtn').addEventListener('click', () => {
    if (answered) { newTask(); return; }
    const val = inputEl.value.trim();
    if (val === '') return;
    answered = true;
    total++;

    if (parseInt(val) === correct) {
      score++; streak++;
      inputEl.classList.add('ok');
      msgEl.textContent = '✅ Верно!';
      msgEl.className = 'msg ok';
    } else {
      streak = 0;
      inputEl.classList.add('err');
      msgEl.textContent = `❌ Правильно: ${correct}`;
      msgEl.className = 'msg err';
    }
    updateStats();
    document.getElementById('checkBtn').textContent = 'Дальше →';
    setTimeout(() => {
      document.getElementById('checkBtn').textContent = 'Проверить';
      newTask();
    }, 1200);
  });

  inputEl.addEventListener('keydown', (e) => {
    if (e.key === 'Enter') document.getElementById('checkBtn').click();
  });

  document.getElementById('resetBtn').addEventListener('click', () => {
    score = 0; total = 0; streak = 0;
    updateStats();
    newTask();
  });

  newTask();
</script>
</body>
</html>
