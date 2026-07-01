
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Field Notes — Daily Tasks</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,400;0,9..144,600;1,9..144,500&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg: #1B1B1D;
    --surface: #242426;
    --surface-2: #2C2C2F;
    --accent: #E8A33D;
    --text: #F2EFE9;
    --muted: #8A8780;
    --done: #6B9B6E;
    --line: #3A3A3D;
  }

  *{ box-sizing: border-box; }

  body{
    margin:0;
    background: var(--bg);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    min-height: 100vh;
    display:flex;
    align-items:flex-start;
    justify-content:center;
    padding: 56px 20px;
  }

  .sheet{
    width: 100%;
    max-width: 620px;
  }

  .masthead{
    display:flex;
    justify-content:space-between;
    align-items:flex-end;
    border-bottom: 2px solid var(--line);
    padding-bottom: 18px;
    margin-bottom: 28px;
  }

  .masthead h1{
    font-family: 'Fraunces', serif;
    font-weight: 600;
    font-style: italic;
    font-size: 2.4rem;
    margin: 0 0 4px 0;
    letter-spacing: -0.5px;
  }

  .masthead .date{
    color: var(--muted);
    font-size: 0.78rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .stat{
    text-align:right;
  }

  .stat .num{
    font-family: 'Fraunces', serif;
    font-size: 2.4rem;
    font-weight: 600;
    color: var(--accent);
    line-height:1;
  }

  .stat .label{
    color: var(--muted);
    font-size: 0.7rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .entry-row{
    display:flex;
    gap:10px;
    margin-bottom: 26px;
  }

  .entry-row input[type=text]{
    flex:1;
    background: var(--surface);
    border: 1px solid var(--line);
    color: var(--text);
    padding: 13px 14px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.9rem;
    border-radius: 3px;
    outline: none;
    transition: border-color 0.15s;
  }

  .entry-row input[type=text]:focus{
    border-color: var(--accent);
  }

  .entry-row input[type=text]::placeholder{
    color: var(--muted);
  }

  .entry-row button{
    background: var(--accent);
    border: none;
    color: #1B1B1D;
    font-family: 'JetBrains Mono', monospace;
    font-weight: 700;
    font-size: 0.8rem;
    letter-spacing: 0.04em;
    padding: 0 20px;
    border-radius: 3px;
    cursor: pointer;
    text-transform: uppercase;
    transition: opacity 0.15s;
  }

  .entry-row button:hover{ opacity: 0.85; }

  .filters{
    display:flex;
    gap: 4px;
    margin-bottom: 18px;
  }

  .filters button{
    background: none;
    border: 1px solid var(--line);
    color: var(--muted);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.7rem;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    padding: 6px 12px;
    border-radius: 3px;
    cursor: pointer;
    transition: all 0.15s;
  }

  .filters button.active{
    color: var(--accent);
    border-color: var(--accent);
  }

  .ticket{
    position: relative;
    display:flex;
    align-items:center;
    gap: 14px;
    background: var(--surface);
    border: 1px solid var(--line);
    border-radius: 4px;
    padding: 15px 16px;
    margin-bottom: 10px;
    animation: appear 0.25s ease;
  }

  @keyframes appear{
    from{ opacity:0; transform: translateY(-4px); }
    to{ opacity:1; transform: translateY(0); }
  }

  .ticket::before{
    content:"";
    position:absolute;
    left:0; top:0; bottom:0;
    width: 3px;
    background: var(--line);
    border-radius: 4px 0 0 4px;
    transition: background 0.15s;
  }

  .ticket.done::before{ background: var(--done); }

  .check{
    width: 20px;
    height: 20px;
    flex-shrink:0;
    border: 2px solid var(--muted);
    border-radius: 50%;
    cursor: pointer;
    display:flex;
    align-items:center;
    justify-content:center;
    transition: border-color 0.15s, background 0.15s;
  }

  .check.done{
    background: var(--done);
    border-color: var(--done);
  }

  .check.done::after{
    content:"✓";
    color: #1B1B1D;
    font-size: 0.7rem;
    font-weight:700;
  }

  .ticket-text{
    flex:1;
    font-size: 0.9rem;
    line-height:1.4;
    word-break: break-word;
  }

  .ticket.done .ticket-text{
    color: var(--muted);
    text-decoration: line-through;
  }

  .ticket-index{
    color: var(--muted);
    font-size: 0.7rem;
    min-width: 22px;
  }

  .del{
    background:none;
    border:none;
    color: var(--muted);
    font-size: 1rem;
    cursor:pointer;
    opacity: 0;
    transition: opacity 0.15s, color 0.15s;
    padding: 4px;
  }

  .ticket:hover .del{ opacity: 1; }
  .del:hover{ color: #D96C5F; }

  .empty{
    text-align:center;
    color: var(--muted);
    font-size: 0.85rem;
    padding: 50px 0;
    border: 1px dashed var(--line);
    border-radius: 4px;
  }

  .empty .glyph{
    font-family:'Fraunces', serif;
    font-style: italic;
    font-size: 1.6rem;
    color: var(--accent);
    display:block;
    margin-bottom: 8px;
  }

  footer{
    margin-top: 30px;
    text-align:center;
    color: var(--line);
    font-size: 0.65rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }
</style>
</head>
<body>

<div class="sheet">
  <div class="masthead">
    <div>
      <h1>Field Notes</h1>
      <div class="date" id="today"></div>
    </div>
    <div class="stat">
      <div class="num" id="remaining">0</div>
      <div class="label">open</div>
    </div>
  </div>

  <div class="entry-row">
    <input type="text" id="taskInput" placeholder="Log a task and press enter…" autofocus>
    <button id="addBtn">Add</button>
  </div>

  <div class="filters">
    <button class="filter-btn active" data-filter="all">All</button>
    <button class="filter-btn" data-filter="open">Open</button>
    <button class="filter-btn" data-filter="done">Done</button>
  </div>

  <div id="list"></div>

  <footer>entry log · sorted by time added</footer>
</div>

<script>
  let tasks = [];
  let filter = 'all';
  let idCounter = 0;

  const listEl = document.getElementById('list');
  const input = document.getElementById('taskInput');
  const remainingEl = document.getElementById('remaining');

  document.getElementById('today').textContent = new Date().toLocaleDateString('en-US', {
    weekday: 'long', month: 'long', day: 'numeric'
  });

  function addTask(){
    const val = input.value.trim();
    if(!val) return;
    tasks.push({ id: idCounter++, text: val, done: false });
    input.value = '';
    render();
  }

  function toggleTask(id){
    const t = tasks.find(t => t.id === id);
    if(t) t.done = !t.done;
    render();
  }

  function deleteTask(id){
    tasks = tasks.filter(t => t.id !== id);
    render();
  }

  function render(){
    let visible = tasks;
    if(filter === 'open') visible = tasks.filter(t => !t.done);
    if(filter === 'done') visible = tasks.filter(t => t.done);

    remainingEl.textContent = tasks.filter(t => !t.done).length;

    if(visible.length === 0){
      listEl.innerHTML = `<div class="empty"><span class="glyph">nothing here yet</span>add a task above to start today's log</div>`;
      return;
    }

    listEl.innerHTML = visible.map((t, i) => `
      <div class="ticket ${t.done ? 'done' : ''}">
        <span class="ticket-index">${String(i+1).padStart(2,'0')}</span>
        <div class="check ${t.done ? 'done' : ''}" onclick="toggleTask(${t.id})"></div>
        <div class="ticket-text">${escapeHtml(t.text)}</div>
        <button class="del" onclick="deleteTask(${t.id})">✕</button>
      </div>
    `).join('');
  }

  function escapeHtml(str){
    const d = document.createElement('div');
    d.textContent = str;
    return d.innerHTML;
  }

  document.getElementById('addBtn').addEventListener('click', addTask);
  input.addEventListener('keydown', e => { if(e.key === 'Enter') addTask(); });

  document.querySelectorAll('.filter-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      filter = btn.dataset.filter;
      render();
    });
  });

  // seed a couple of example entries
  tasks.push({ id: idCounter++, text: 'Sketch layout for the new page', done: true });
  tasks.push({ id: idCounter++, text: 'Reply to client feedback', done: false });
  render();
</script>

</body>
</html>
