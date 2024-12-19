---
name: User Story
about: This template is for creating user stories
title: ''
labels: ''
assignees: ''

---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kanban Board</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f4f4f4;
        }
        .kanban-board {
            display: flex;
            width: 80%;
            max-width: 1200px;
        }
        .kanban-column {
            flex: 1;
            margin: 0 10px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        .kanban-column h2 {
            text-align: center;
            background-color: #007bff;
            color: #fff;
            margin: 0;
            padding: 10px;
            border-radius: 5px 5px 0 0;
        }
        .kanban-items {
            padding: 10px;
            min-height: 200px;
            border: 2px dashed #ddd;
            border-radius: 5px;
        }
        .kanban-item {
            background-color: #e2e2e2;
            margin: 10px 0;
            padding: 10px;
            border-radius: 5px;
            cursor: grab;
        }
        .kanban-item:active {
            cursor: grabbing;
        }
        .add-task {
            margin: 10px;
            text-align: center;
        }
        .add-task input {
            width: 80%;
            padding: 5px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .add-task button {
            padding: 5px 10px;
            margin-top: 5px;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        .add-task button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <div class="kanban-board">
        <div class="kanban-column">
            <h2>To Do</h2>
            <div class="kanban-items" data-column="todo">
                <div class="kanban-item" draggable="true">Task 1</div>
                <div class="kanban-item" draggable="true">Task 2</div>
            </div>
            <div class="add-task">
                <input type="text" placeholder="New Task">
                <button onclick="addTask(this)">Add Task</button>
            </div>
        </div>
        <div class="kanban-column">
            <h2>In Progress</h2>
            <div class="kanban-items" data-column="inprogress"></div>
            <div class="add-task">
                <input type="text" placeholder="New Task">
                <button onclick="addTask(this)">Add Task</button>
            </div>
        </div>
        <div class="kanban-column">
            <h2>Done</h2>
            <div class="kanban-items" data-column="done"></div>
            <div class="add-task">
                <input type="text" placeholder="New Task">
                <button onclick="addTask(this)">Add Task</button>
            </div>
        </div>
    </div>

    <script>
        // Drag-and-Drop Functionality
        const items = document.querySelectorAll('.kanban-item');
        const columns = document.querySelectorAll('.kanban-items');

        items.forEach(item => {
            item.addEventListener('dragstart', dragStart);
            item.addEventListener('dragend', dragEnd);
        });

        columns.forEach(column => {
            column.addEventListener('dragover', dragOver);
            column.addEventListener('drop', drop);
        });

        function dragStart(e) {
            e.dataTransfer.setData('text/plain', e.target.outerHTML);
            e.target.classList.add('dragging');
        }

        function dragEnd(e) {
            e.target.classList.remove('dragging');
        }

        function dragOver(e) {
            e.preventDefault();
        }

        function drop(e) {
            e.preventDefault();
            const data = e.dataTransfer.getData('text/plain');
            const draggedElement = document.createElement('div');
            draggedElement.outerHTML = data;
            e.target.appendChild(draggedElement);
            const original = document.querySelector('.dragging');
            original.remove();
        }

        // Add Task Functionality
        function addTask(button) {
            const input = button.previousElementSibling;
            const taskText = input.value.trim();
            if (taskText) {
                const task = document.createElement('div');
                task.className = 'kanban-item';
                task.draggable = true;
                task.textContent = taskText;

                // Add drag-and-drop listeners
                task.addEventListener('dragstart', dragStart);
                task.addEventListener('dragend', dragEnd);

                const column = button.closest('.kanban-column').querySelector('.kanban-items');
                column.appendChild(task);
                input.value = '';
            }
        }
    </script>
</body>
</html>
