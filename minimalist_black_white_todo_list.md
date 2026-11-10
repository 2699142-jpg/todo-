```
<title>Minimalist Todo List</title>
<!-- Combined CSS Styles (style.css logic inside standard tag for single-file preview capability) -->
<style>
    /* Base Styles & CSS Variables - Soft Pastel Theme */
    :root 
        --bg-gradient: linear-gradient(135deg, #e0c3fc 0%, #8ec5fc 100%);
        --card-bg: rgba(255, 255, 255, 0.88);
        --card-border: rgba(255, 255, 255, 0.6);
        --text-main: #2d3748;
        --text-muted: #718096;
        --border-color: #e2e8f0;
        --accent-purple: #9f7aea;
        --accent-purple-hover: #805ad5;
        --item-bg: #ffffff;
        --item-hover-border: #cbd5e0;
        --completed-text: #a0aec0;
        --font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }

    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
        font-family: var(--font-family);
        -webkit-font-smoothing: antialiased;
    }

    body {
        background: var(--bg-gradient);
        color: var(--text-main);
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-items: flex-start;
        padding: 60px 20px;
    }

    /* Container Layout */
    .app-container {
        width: 100%;
        max-width: 520px;
        display: flex;
        flex-direction: column;
        gap: 20px;
        background: var(--card-bg);
        backdrop-filter: blur(16px);
        border: 1px solid var(--card-border);
        border-radius: 20px;
        padding: 32px 28px;
        box-shadow: 0 15px 35px rgba(103, 110, 144, 0.15);
    }

    /* Header */
    header {
        display: flex;
        justify-content: space-between;
        align-items: baseline;
        border-bottom: 2px solid #edf2f7;
        padding-bottom: 16px;
    }

    h1 {
        font-size: 1.8rem;
        font-weight: 800;
        letter-spacing: -0.02em;
        color: #4a5568;
    }

    .task-count {
        font-size: 0.85rem;
        color: var(--text-muted);
        font-weight: 600;
    }

    /* Input Form */
    .input-form {
        display: flex;
        gap: 10px;
    }

    .input-form input {
        flex: 1;
        background: #ffffff;
        border: 1.5px solid var(--border-color);
        border-radius: 12px;
        padding: 14px 16px;
        color: var(--text-main);
        font-size: 0.95rem;
        outline: none;
        transition: all 0.2s ease;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.02);
    }

    .input-form input:focus {
        border-color: var(--accent-purple);
        box-shadow: 0 0 0 3px rgba(159, 122, 234, 0.2);
    }

    .input-form input::placeholder {
        color: #a0aec0;
    }

    .input-form button {
        background-color: var(--accent-purple);
        color: #ffffff;
        border: none;
        border-radius: 12px;
        padding: 0 20px;
        font-weight: 700;
        font-size: 1.3rem;
        cursor: pointer;
        transition: all 0.2s ease;
        box-shadow: 0 4px 12px rgba(159, 122, 234, 0.3);
    }

    .input-form button:hover {
        background-color: var(--accent-purple-hover);
        transform: translateY(-1px);
    }

    .input-form button:active {
        transform: translateY(0);
    }

    /* Filter Controls */
    .controls-bar {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 12px;
        font-size: 0.85rem;
    }

    .filters {
        display: flex;
        gap: 6px;
        background: #edf2f7;
        padding: 4px;
        border-radius: 10px;
    }

    .filter-btn {
        background: none;
        border: none;
        color: var(--text-muted);
        padding: 6px 14px;
        border-radius: 8px;
        cursor: pointer;
        font-size: 0.8rem;
        font-weight: 600;
        transition: all 0.2s ease;
    }

    .filter-btn:hover {
        color: var(--text-main);
    }

    .filter-btn.active {
        color: var(--accent-purple-hover);
        background-color: #ffffff;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.06);
    }

    .clear-btn {
        background: none;
        border: none;
        color: var(--text-muted);
        cursor: pointer;
        font-size: 0.8rem;
        font-weight: 600;
        transition: color 0.2s ease;
    }

    .clear-btn:hover {
        color: #e53e3e;
        text-decoration: underline;
    }

    /* Todo List Elements */
    .todo-list {
        list-style: none;
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    .todo-item {
        display: flex;
        align-items: center;
        justify-content: space-between;
        background-color: var(--item-bg);
        border: 1px solid var(--border-color);
        border-radius: 12px;
        padding: 14px 16px;
        transition: all 0.2s ease;
        box-shadow: 0 2px 6px rgba(0, 0, 0, 0.02);
        animation: fadeIn 0.2s ease forwards;
    }

    @keyframes fadeIn {
        from {
            opacity: 0;
            transform: translateY(-4px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }

    .todo-item:hover {
        border-color: var(--item-hover-border);
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.04);
    }

    .todo-content {
        display: flex;
        align-items: center;
        gap: 12px;
        flex: 1;
        cursor: pointer;
        overflow: hidden;
    }

    /* Custom Checkbox */
    .checkbox {
        width: 20px;
        height: 20px;
        border: 2px solid #cbd5e0;
        border-radius: 6px;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0;
        transition: all 0.2s ease;
    }

    .todo-item.completed .checkbox {
        background-color: #cbd5e0;
        border-color: #cbd5e0;
    }

    .checkbox::after {
        content: "";
        width: 5px;
        height: 9px;
        border: solid #ffffff;
        border-width: 0 2px 2px 0;
        transform: rotate(45deg);
        display: none;
        margin-bottom: 2px;
    }

    .todo-item.completed .checkbox::after {
        display: block;
    }

    /* Text Display */
    .todo-text {
        font-size: 0.95rem;
        color: var(--text-main);
        word-break: break-all;
        transition: color 0.2s ease;
    }

    .todo-item.completed .todo-text {
        color: var(--completed-text);
        text-decoration: line-through;
    }

    /* Delete Button */
    .delete-btn {
        background: none;
        border: none;
        color: #cbd5e0;
        cursor: pointer;
        font-size: 1.2rem;
        line-height: 1;
        padding: 4px 8px;
        border-radius: 6px;
        opacity: 0.7;
        transition: all 0.2s ease;
        margin-left: 8px;
    }

    .todo-item:hover .delete-btn {
        opacity: 1;
    }

    .delete-btn:hover {
        color: #e53e3e;
        background-color: #fff5f5;
    }

    /* Empty State */
    .empty-state {
        text-align: center;
        padding: 36px 0;
        color: var(--text-muted);
        font-size: 0.9rem;
        border: 2px dashed #e2e8f0;
        border-radius: 12px;
        background: rgba(255, 255, 255, 0.4);
    }

    /* Footer Badge */
    .footer-badge-container {
        display: flex;
        justify-content: flex-end;
        margin-top: 8px;
    }

    .items-badge {
        display: inline-flex;
        align-items: center;
        gap: 8px;
        background-color: #f3e8ff;
        color: #6b21a8;
        padding: 6px 14px;
        border-radius: 20px;
        font-size: 0.85rem;
        font-weight: 700;
        letter-spacing: 0.3px;
        box-shadow: 0 2px 6px rgba(159, 122, 234, 0.15);
    }

    .badge-dot {
        width: 7px;
        height: 7px;
        background-color: #9333ea;
        border-radius: 50%;
    }

    /* Toast Notifications */
    .toast {
        position: fixed;
        bottom: 24px;
        left: 50%;
        transform: translateX(-50%) translateY(100px);
        background-color: #2d3748;
        color: #ffffff;
        padding: 10px 20px;
        border-radius: 8px;
        font-size: 0.85rem;
        font-weight: 600;
        opacity: 0;
        transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275), opacity 0.3s ease;
        pointer-events: none;
        box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
    }

    .toast.show {
        transform: translateX(-50%) translateY(0);
        opacity: 1;
    }
</style>

```

```
<main class="app-container">
    <!-- Header Section -->
    <header>
        <h1>TASKS</h1>
        <span class="task-count" id="task-count">남은 항목: 0개</span>
    </header>

    <!-- Task Input Section -->
    <form class="input-form" id="todo-form">
        <input 
            type="text" 
            id="todo-input" 
            placeholder="새로운 할 일을 입력하세요..." 
            autocomplete="off"
            maxlength="100"
        />
        <button type="submit" aria-label="Add Task">+</button>
    </form>

    <!-- Filters & Action Bar -->
    <div class="controls-bar">
        <div class="filters">
            <button class="filter-btn active" data-filter="all">전체</button>
            <button class="filter-btn" data-filter="active">진행 중</button>
            <button class="filter-btn" data-filter="completed">완료됨</button>
        </div>
        <button class="clear-btn" id="clear-completed">완료 항목 삭제</button>
    </div>

    <!-- Todo Items Container -->
    <ul class="todo-list" id="todo-list"></ul>

    <!-- Empty State Container -->
    <div class="empty-state" id="empty-state" style="display: none;">
        등록된 할 일이 없습니다.
    </div>

    <!-- Highlighted Counter Badge at Bottom -->
    <footer class="footer-badge-container">
        <div class="items-badge">
            <span class="badge-dot"></span>
            <span id="items-badge-count">남은 항목: 0개</span>
        </div>
    </footer>
</main>

<!-- UI Notification Toast -->
<div class="toast" id="toast"></div>

<!-- Combined JavaScript -->
<script>
    document.addEventListener('DOMContentLoaded', () => {
        // DOM Elements
        const todoForm = document.getElementById('todo-form');
        const todoInput = document.getElementById('todo-input');
        const todoList = document.getElementById('todo-list');
        const emptyState = document.getElementById('empty-state');
        const taskCount = document.getElementById('task-count');
        const itemsBadgeCount = document.getElementById('items-badge-count');
        const filterBtns = document.querySelectorAll('.filter-btn');
        const clearCompletedBtn = document.getElementById('clear-completed');
        const toast = document.getElementById('toast');

        // Application State
        let todos = JSON.parse(localStorage.getItem('bw_todos')) || [];
        let currentFilter = 'all';

        // Save state to LocalStorage
        function saveTodos() {
            localStorage.setItem('bw_todos', JSON.stringify(todos));
        }

        // Show temporary toast message
        function showToast(message) {
            toast.textContent = message;
            toast.classList.add('show');
            setTimeout(() => {
                toast.classList.remove('show');
            }, 2000);
        }

        // Render tasks according to active filter
        function render() {
            todoList.innerHTML = '';

            // Filter tasks based on active selection
            const filteredTodos = todos.filter(todo => {
                if (currentFilter === 'active') return !todo.completed;
                if (currentFilter === 'completed') return todo.completed;
                return true;
            });

            // Toggle empty state visibility
            if (filteredTodos.length === 0) {
                emptyState.style.display = 'block';
            } else {
                emptyState.style.display = 'none';
            }

            // Render list items
            filteredTodos.forEach(todo => {
                const li = document.createElement('li');
                li.className = `todo-item ${todo.completed ? 'completed' : ''}`;
                li.dataset.id = todo.id;

                li.innerHTML = `
                    <div class="todo-content">
                        <div class="checkbox"></div>
                        <span class="todo-text"></span>
                    </div>
                    <button class="delete-btn" aria-label="Delete Task">&times;</button>
                `;

                // Safely insert user text to prevent XSS
                li.querySelector('.todo-text').textContent = todo.text;

                // Toggle status click handler
                li.querySelector('.todo-content').addEventListener('click', () => {
                    toggleTodo(todo.id);
                });

                // Delete item click handler
                li.querySelector('.delete-btn').addEventListener('click', (e) => {
                    e.stopPropagation();
                    deleteTodo(todo.id);
                });

                todoList.appendChild(li);
            });

            // Update items counter
            const activeCount = todos.filter(t => !t.completed).length;
            const countText = `남은 항목: ${activeCount}개`;
            taskCount.textContent = countText;
            itemsBadgeCount.textContent = countText;
        }

        // Add new task handler
        function addTodo(text) {
            const trimmedText = text.trim();
            if (!trimmedText) return;

            const newTodo = {
                id: Date.now().toString(),
                text: trimmedText,
                completed: false
            };

            todos.unshift(newTodo);
            saveTodos();
            render();
            todoInput.value = '';
            showToast('할 일이 추가되었습니다');
        }

        // Toggle task completed state
        function toggleTodo(id) {
            todos = todos.map(todo => {
                if (todo.id === id) {
                    return { ...todo, completed: !todo.completed };
                }
                return todo;
            });
            saveTodos();
            render();
        }

        // Delete task handler
        function deleteTodo(id) {
            todos = todos.filter(todo => todo.id !== id);
            saveTodos();
            render();
            showToast('할 일이 삭제되었습니다');
        }

        // Clear completed tasks handler
        function clearCompleted() {
            const completedCount = todos.filter(t => t.completed).length;
            if (completedCount === 0) {
                showToast('완료된 항목이 없습니다');
                return;
            }
            
            todos = todos.filter(todo => !todo.completed);
            saveTodos();
            render();
            showToast('완료된 항목을 삭제했습니다');
        }

        // Event Listeners
        todoForm.addEventListener('submit', (e) => {
            e.preventDefault();
            addTodo(todoInput.value);
        });

        filterBtns.forEach(btn => {
            btn.addEventListener('click', () => {
                filterBtns.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                currentFilter = btn.dataset.filter;
                render();
            });
        });

        clearCompletedBtn.addEventListener('click', clearCompleted);

        // Initial Render
        render();
    });
</script>

```