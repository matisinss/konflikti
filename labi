<!-- Save this as todo.html and open in your browser -->
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Simple One-File To-Do</title>
  <style>
    :root{font-family:system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;}
    body{max-width:720px;margin:36px auto;padding:18px;background:#f7f7f9;border-radius:12px}
    h1{margin:0 0 12px;font-size:24px}
    .row{display:flex;gap:8px;margin-bottom:12px}
    input[type="text"]{flex:1;padding:8px;border-radius:8px;border:1px solid #ddd}
    button{padding:8px 12px;border-radius:8px;border:none;cursor:pointer}
    ul{list-style:none;padding:0;margin:0}
    li{display:flex;align-items:center;gap:12px;padding:8px;border-radius:8px;margin-bottom:6px;background:#fff;box-shadow:0 1px 0 rgba(0,0,0,0.04)}
    li.completed .text{opacity:.5;text-decoration:line-through}
    .text{flex:1}
    .small{font-size:12px;color:#666}
    .filters{display:flex;gap:8px;margin-top:12px}
    .btn-ghost{background:#fff;border:1px solid #ddd}
  </style>
</head>
<body>
  <h1>Simple To-Do (one file)</h1>

  <div class="row">
    <input id="newTask" type="text" placeholder="Add a task and press Enter..." />
    <button id="addBtn">Add</button>
  </div>

  <div class="row small">
    <div id="stats">0 tasks</div>
    <div style="flex:1"></div>
    <button id="clearCompleted" class="btn-ghost">Clear completed</button>
    <button id="clearAll" class="btn-ghost">Clear all</button>
  </div>

  <ul id="list"></ul>

  <div class="filters">
    <button data-filter="all" class="btn-ghost">All</button>
    <button data-filter="active" class="btn-ghost">Active</button>
    <button data-filter="completed" class="btn-ghost">Completed</button>
  </div>

<script>
(() => {
  // Simple model stored in localStorage
  const KEY = 'simple_todo_v1';
  let tasks = JSON.parse(localStorage.getItem(KEY) || '[]');
  let filter = 'all';

  // DOM refs
  const newTask = document.getElementById('newTask');
  const addBtn = document.getElementById('addBtn');
  const list = document.getElementById('list');
  const stats = document.getElementById('stats');
  const clearCompleted = document.getElementById('clearCompleted');
  const clearAll = document.getElementById('clearAll');
  const filterBtns = document.querySelectorAll('[data-filter]');

  function save() {
    localStorage.setItem(KEY, JSON.stringify(tasks));
    render();
  }

  function addTask(text) {
    if (!text.trim()) return;
    tasks.push({ id: Date.now(), text: text.trim(), done: false });
    save();
    newTask.value = '';
    newTask.focus();
  }

  function removeTask(id) {
    tasks = tasks.filter(t => t.id !== id);
    save();
  }

  function toggleDone(id) {
    tasks = tasks.map(t => t.id === id ? { ...t, done: !t.done } : t);
    save();
  }

  function clearCompletedTasks() {
    tasks = tasks.filter(t => !t.done);
    save();
  }

  function clearAllTasks() {
    if (!confirm('Clear ALL tasks?')) return;
    tasks = [];
    save();
  }

  function setFilter(f) {
    filter = f;
    filterBtns.forEach(b => b.style.fontWeight = b.dataset.filter === f ? '700' : '400');
    render();
  }

  function render() {
    // apply filter
    const shown = tasks.filter(t => filter === 'all' ? true : filter === 'active' ? !t.done : t.done);
    list.innerHTML = '';
    shown.forEach(t => {
      const li = document.createElement('li');
      li.className = t.done ? 'completed' : '';
      li.innerHTML = `
        <input type="checkbox" ${t.done ? 'checked' : ''} />
        <div class="text">${escapeHtml(t.text)}</div>
        <button class="btn-ghost btn-edit small">Edit</button>
        <button class="btn-ghost btn-remove small">Delete</button>
      `;
      // events
      li.querySelector('input[type="checkbox"]').addEventListener('change', () => toggleDone(t.id));
      li.querySelector('.btn-remove').addEventListener('click', () => removeTask(t.id));
      li.querySelector('.btn-edit').addEventListener('click', () => {
        const newText = prompt('Edit task', t.text);
        if (newText === null) return;
        tasks = tasks.map(x => x.id === t.id ? { ...x, text: newText.trim() } : x);
        save();
      });
      list.appendChild(li);
    });

    // stats
    const total = tasks.length;
    const remaining = tasks.filter(t => !t.done).length;
    stats.textContent = `${remaining} / ${total} active`;
  }

  function escapeHtml(s) {
    return s.replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":"&#39;"}[c]));
  }

  // events
  addBtn.addEventListener('click', () => addTask(newTask.value));
  newTask.addEventListener('keydown', e => { if (e.key === 'Enter') addTask(newTask.value); });
  clearCompleted.addEventListener('click', clearCompletedTasks);
  clearAll.addEventListener('click', clearAllTasks);
  filterBtns.forEach(b => b.addEventListener('click', () => setFilter(b.dataset.filter)));

  // initial render
  setFilter('all');
})();
</script>
</body>
</html>
