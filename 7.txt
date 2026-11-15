document.addEventListener('DOMContentLoaded', () => {
    
    // ==========================================
    // 1. ACADEMIC PLANNER FUNCTIONALITY
    // ==========================================
    const taskInput = document.getElementById('taskInput');
    const addTaskBtn = document.getElementById('addTaskBtn');
    const taskList = document.getElementById('taskList');

    if (taskInput && addTaskBtn && taskList) {
        // Array to store tasks
        let tasks = JSON.parse(localStorage.getItem('academicTasks')) || [
            { text: 'Submit COS 106 Web Technologies Project', completed: false }
        ];

        function saveAndRenderTasks() {
            localStorage.setItem('academicTasks', JSON.stringify(tasks));
            taskList.innerHTML = '';

            tasks.forEach((task, index) => {
                const li = document.createElement('li');
                li.className = `task-item ${task.completed ? 'completed' : ''}`;

                const span = document.createElement('span');
                span.textContent = task.text;

                const actionsDiv = document.createElement('div');
                actionsDiv.className = 'task-actions';

                const completeBtn = document.createElement('button');
                completeBtn.textContent = task.completed ? 'Undo' : 'Complete';
                completeBtn.className = 'complete-btn';
                completeBtn.addEventListener('click', () => toggleTask(index));

                const deleteBtn = document.createElement('button');
                deleteBtn.textContent = 'Delete';
                deleteBtn.className = 'delete-btn';
                deleteBtn.addEventListener('click', () => deleteTask(index));

                actionsDiv.appendChild(completeBtn);
                actionsDiv.appendChild(deleteBtn);
                li.appendChild(span);
                li.appendChild(actionsDiv);
                taskList.appendChild(li);
            });
        }

        function addTask() {
            const text = taskInput.value.trim();
            if (text === '') {
                alert('Please enter a task description.');
                return;
            }
            tasks.push({ text: text, completed: false });
            taskInput.value = '';
            saveAndRenderTasks();
        }

        function toggleTask(index) {
            tasks[index].completed = !tasks[index].completed;
            saveAndRenderTasks();
        }

        function deleteTask(index) {
            tasks.splice(index, 1);
            saveAndRenderTasks();
        }

        addTaskBtn.addEventListener('click', addTask);
        taskInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') addTask();
        });

        // Initial render
        saveAndRenderTasks();
    }

    // ==========================================
    // 2. CONTACT FORM VALIDATION
    // ==========================================
    const contactForm = document.getElementById('contactForm');
    const formFeedback = document.getElementById('formFeedback');

    if (contactForm) {
        contactForm.addEventListener('submit', (e) => {
            e.preventDefault();

            const name = document.getElementById('name').value.trim();
            const email = document.getElementById('email').value.trim();
            const phone = document.getElementById('phone').value.trim();
            const message = document.getElementById('message').value.trim();

            // 1. Check no field is empty
            if (!name || !email || !phone || !message) {
                showFeedback('Error: All fields are required.', 'red');
                return;
            }

            // 2. Validate email format
            const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!emailPattern.test(email)) {
                showFeedback('Error: Please enter a valid email address.', 'red');
                return;
            }

            // 3. Validate phone number contains only digits
            const digitsOnlyPattern = /^\d+$/;
            if (!digitsOnlyPattern.test(phone)) {
                showFeedback('Error: Phone number must contain only digits.', 'red');
                return;
            }

            // Success
            showFeedback('Success! Your message has been validated and sent.', 'green');
            contactForm.reset();
        });

        function showFeedback(msg, color) {
            formFeedback.textContent = msg;
            formFeedback.style.color = color;
        }
    }
});
