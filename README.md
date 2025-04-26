<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Form Validation with Responsive Layout & To-Do List</title>
<style>
  /* Reset some default styles */
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: Arial, sans-serif;
    /* Elegant background image */
    background-image: url('https://images.unsplash.com/photo-1506744038136-46273834b3fb?ixlib=rb-4.0.3&auto=format&fit=crop&w=1600&q=80');
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    padding: 20px;
  }

  /* Navigation Bar using Flexbox */
  nav {
    display: flex;
    justify-content: space-around;
    background-color: rgba(51, 51, 51, 0.7);
    padding: 10px 0;
    margin-bottom: 20px;
  }

  nav a {
    color: whitesmoke;
    text-decoration: none;
    padding: 8px 16px;
  }

  nav a:hover {
    background-color: #575757;
  }

  /* Container using CSS Grid for layout */
  .main-container {
    display: grid;
    grid-template-columns: 1fr;
    gap: 20px;
  }

  /* Media query for larger screens */
  @media (min-width: 768px) {
    .main-container {
      grid-template-columns: 2fr 1fr; /* Content and sidebar */
    }
  }

  /* Form and To-Do List styles */
  .content-area {
    background-color: rgba(255, 255, 255, 0.85);
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }

  /* To-Do List styles */
  .todo-section {
    margin-top: 50px;
  }

  h2 {
    text-align: center;
    margin-bottom: 20px;
  }

  /* Existing form styles */
  .form-container {
    max-width: 400px;
    margin: auto;
  }

  label {
    display: block;
    margin-top: 15px;
    font-weight: bold;
  }

  input[type="text"], input[type="email"], input[type="tel"] {
    width: 100%;
    padding: 8px 10px;
    margin-top: 5px;
    box-sizing: border-box;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .error {
    color: red;
    font-size: 0.9em;
    margin-top: 5px;
  }

  button {
    margin-top: 20px;
    width: 100%;
    padding: 10px;
    background-color: #300909;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 1em;
  }

  button:hover {
    background-color: #440505;
  }

  /* To-Do List styles */
  #todoInput {
    width: calc(100% - 22px);
    padding: 8px;
    margin-right: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  #addTodoBtn {
    padding: 8px 16px;
    background-color: #440505;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  #addTodoBtn:hover {
    background-color: #45a049;
  }

  .todo-list {
    margin-top: 20px;
    list-style: none;
  }

  .todo-item {
    background-color: #e0e0e0;
    padding: 10px;
    margin-top: 8px;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .remove-btn {
    background-color: #f44336;
    border: none;
    padding: 5px 10px;
    border-radius: 4px;
    color: white;
    cursor: pointer;
  }

  .remove-btn:hover {
    background-color: #d32f2f;
  }
</style>
</head>
<body>

<!-- Navigation Bar using Flexbox -->
<nav>
  <a href="#form-section"><u>Form</u></a>
  <a href="#todo-section"><u>To-Do List</u></a>
</nav>

<!-- Main content using CSS Grid -->
<div class="main-container">

  <!-- Form Section -->
  <div class="content-area" id="form-section">
    <h2>Forms</h2>
    <div class="form-container">
      <form id="registrationForm" novalidate>
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" placeholder="Enter your name" />
        <div id="nameError" class="error"></div>

        <label for="mobile">Mobile:</label>
        <input type="tel" id="mobile" name="mobile" placeholder="Enter 10-digit mobile number" />
        <div id="mobileError" class="error"></div>

        <label for="email">Email:</label>
        <input type="email" id="email" name="email" placeholder="xxxxx@xxxxx.xxx" />
        <div id="emailError" class="error"></div>

        <button type="submit">Submit</button>
      </form>
    </div>
  </div>

  <!-- To-Do List Section -->
  <div class="content-area" id="todo-section" class="todo-section">
    <h2>To-Do List</h2>
    <div style="display:flex; margin-bottom:10px;">
      <input type="text" id="todoInput" placeholder="Add new task" />
      <button id="addTodoBtn">Add</button>
    </div>
    <ul class="todo-list" id="todoList"></ul>
  </div>

</div>

<script>
  // Wait for DOM to load
  document.addEventListener('DOMContentLoaded', function() {
    // --- Form Validation ---
    const form = document.getElementById('registrationForm');
    const nameError = document.getElementById('nameError');
    const mobileError = document.getElementById('mobileError');
    const emailError = document.getElementById('emailError');

    function validateName(name) {
      const namePattern = /^[A-Za-z][A-Za-z0-9]{5,}$/;
      return namePattern.test(name);
    }

    function validateMobile(mobile) {
      const mobilePattern = /^\d{10}$/;
      return mobilePattern.test(mobile);
    }

    function validateEmail(email) {
      const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!emailPattern.test(email)) return false;
      const atIndex = email.indexOf('@');
      const dotIndex = email.lastIndexOf('.');
      if (atIndex < 1 || dotIndex <= atIndex + 1 || dotIndex === email.length - 1) {
        return false;
      }
      return true;
    }

    form.addEventListener('submit', function(e) {
      e.preventDefault();
      // Clear errors
      nameError.textContent = '';
      mobileError.textContent = '';
      emailError.textContent = '';

      const nameVal = document.getElementById('name').value.trim();
      const mobileVal = document.getElementById('mobile').value.trim();
      const emailVal = document.getElementById('email').value.trim();

      let isValid = true;

      if (!validateName(nameVal)) {
        nameError.textContent = 'Name must start with a letter, be alphanumeric, and at least 6 characters.';
        isValid = false;
      }

      if (!validateMobile(mobileVal)) {
        mobileError.textContent = 'Mobile number must be exactly 10 digits.';
        isValid = false;
      }

      if (!validateEmail(emailVal)) {
        emailError.textContent = 'Enter a valid email format like xxxxxxx@xxxxxx.xxx';
        isValid = false;
      }

      if (isValid) {
        alert('Registration successful!');
        form.reset();
      }
    });

    // --- To-Do List Functionality ---
    const todoInput = document.getElementById('todoInput');
    const addBtn = document.getElementById('addTodoBtn');
    const todoList = document.getElementById('todoList');

    function createTodoItem(taskText) {
      const li = document.createElement('li');
      li.className = 'todo-item';

      const span = document.createElement('span');
      span.textContent = taskText;

      const removeBtn = document.createElement('button');
      removeBtn.textContent = 'Remove';
      removeBtn.className = 'remove-btn';

      // Remove task on click
      removeBtn.addEventListener('click', function() {
        todoList.removeChild(li);
      });

      li.appendChild(span);
      li.appendChild(removeBtn);
      return li;
    }

    addBtn.addEventListener('click', function() {
      const task = todoInput.value.trim();
      if (task !== '') {
        const li = createTodoItem(task);
        todoList.appendChild(li);
        todoInput.value = '';
        todoInput.focus();
      }
    });

    // Optional: allow pressing Enter to add task
    todoInput.addEventListener('keydown', function(e) {
      if (e.key === 'Enter') {
        e.preventDefault();
        addBtn.click();
      }
    });
  });
</script>

</body>
</html>
