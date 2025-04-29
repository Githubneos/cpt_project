---
layout: post
title: Taskmanager 
search_exclude: true
description: Manage you tasks
hide: true
menu: nav/home.html
---

<div class="container">
  <h2>📚 Smart Study Scheduler</h2>

  <!-- ✅ INPUT from user -->
  <input id="taskName" placeholder="Task Name" />
  <input id="timeNeeded" placeholder="Estimated Time (min)" type="number" />
  <input id="deadline" type="datetime-local" />
  <button onclick="handleNewTask()">Add Task</button>

  <!-- ✅ OUTPUT -->
  <h3>🧠 Suggested Task:</h3>
  <p id="suggestedTask">No tasks yet.</p>

  <h3>📅 Task List</h3>
  <!-- ✅ Table for better visual output -->
  <table id="taskTable" border="1" style="width:100%; margin-top: 10px; border-collapse: collapse;">
    <thead>
      <tr>
        <th>Task</th>
        <th>Time (min)</th>
        <th>Deadline</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>
</div>

<style>
  body {
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(135deg, #d4fc79, #96e6a1);
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .container {
    background: white;
    padding: 25px;
    border-radius: 16px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
    width: 500px;
    text-align: center;
  }

  input, button {
    width: 100%;
    margin: 5px 0;
    padding: 10px;
    border-radius: 10px;
    border: none;
  }

  button {
    background-color: #4caf50;
    color: white;
    font-weight: bold;
    cursor: pointer;
  }

  th, td {
    padding: 8px;
    text-align: center;
  }

  th {
    background-color: #f0f0f0;
  }

  td button {
    padding: 5px 10px;
    background-color: #f44336;
    color: white;
    border-radius: 6px;
    border: none;
    cursor: pointer;
  }
</style>

<script>
  // ✅ COLLECTION (list) to store task objects
  let tasks = []; // List of tasks stored as objects

  // ✅ INPUT procedure: gets input and uses parameters
  // **Return Type**: void
  // **Parameters**: None (inputs come from the HTML form)
  function handleNewTask() {
    const name = document.getElementById("taskName").value;
    const time = parseInt(document.getElementById("timeNeeded").value);
    const deadline = new Date(document.getElementById("deadline").value);

    if (!name || isNaN(time) || isNaN(deadline.getTime())) return; // Input validation

    // ✅ Calling with parameters to add the task
    addTask(name, time, deadline);
  }

  // ✅ PROCEDURE to add a task to the list
  // **Return Type**: void
  // **Parameters**: 
  // - name (string): The name of the task
  // - time (number): The estimated time (in minutes) to complete the task
  // - deadline (Date): The deadline of the task
  function addTask(name, time, deadline) {
    const task = { name, time, deadline: deadline.toISOString() };

    tasks.push(task); // Add task to the list

    tasks.sort((a, b) => new Date(a.deadline) - new Date(b.deadline)); // Sort tasks by deadline
    saveTasks(); // Save tasks to local storage
    displayTasks(); // Display tasks on the table
    suggestTask(); // Suggest the next task based on the earliest deadline
  }

  // ✅ PROCEDURE to suggest the task with the earliest deadline
  // **Return Type**: void
  // **Parameters**: None (works with the tasks list)
  function suggestTask() {
    // Sequencing: Check if the task list is empty
    if (tasks.length === 0) {
      document.getElementById("suggestedTask").textContent = "You're all caught up!";
      return; // If no tasks, show a message and exit
    }

    let bestTask = tasks[0]; // Assume the first task is the best task
    let soonestDeadline = new Date(tasks[0].deadline); // Store the first task's deadline

    // Iteration: Go through each task to find the one with the earliest deadline
    for (let i = 1; i < tasks.length; i++) {
      const currentTaskDeadline = new Date(tasks[i].deadline);

      // Selection: If current task has an earlier deadline, update bestTask
      if (currentTaskDeadline < soonestDeadline) {
        bestTask = tasks[i];
        soonestDeadline = currentTaskDeadline;
      }
    }

    // Update the suggested task display
    document.getElementById("suggestedTask").textContent =
      `${bestTask.name} - ${bestTask.time} mins - Due: ${soonestDeadline.toLocaleString()}`;
  }

  // ✅ PROCEDURE to display tasks in a table
  // **Return Type**: void
  // **Parameters**: None (works with the tasks list)
  function displayTasks() {
    const table = document.querySelector("#taskTable tbody");
    table.innerHTML = ""; // Clear the table

    // Iteration: Loop through tasks and add each one to the table
    tasks.forEach((task, index) => {
      const row = document.createElement("tr");

      row.innerHTML = `
        <td>${task.name}</td>
        <td>${task.time}</td>
        <td>${new Date(task.deadline).toLocaleString()}</td>
        <td><button onclick="removeTask(${index})">Done</button></td>
      `;

      table.appendChild(row);
    });
  }

  // ✅ PROCEDURE to remove a task from the list
  // **Return Type**: void
  // **Parameters**: 
  // - index (number): The index of the task to remove
  function removeTask(index) {
    tasks.splice(index, 1); // Remove the task from the list by index
    saveTasks(); // Save the updated tasks
    displayTasks(); // Re-render the task list
    suggestTask(); // Update the suggested task
  }

  // ✅ PROCEDURE to save tasks to local storage
  // **Return Type**: void
  // **Parameters**: None (uses tasks list)
  function saveTasks() {
    localStorage.setItem("studyTasks", JSON.stringify(tasks)); // Save the tasks as a JSON string
  }

  // ✅ PROCEDURE to load tasks from local storage
  // **Return Type**: void
  // **Parameters**: None (loads saved tasks)
  function loadTasks() {
    const saved = localStorage.getItem("studyTasks");
    if (saved) {
      tasks = JSON.parse(saved); // Load tasks from local storage
    } else {
      // ✅ If no saved tasks, load some example tasks
      tasks = [
        { name: "APUSH Chapter 22 Reading", time: 45, deadline: new Date(Date.now() + 2 * 3600000).toISOString() },
        { name: "Physics Worksheet", time: 30, deadline: new Date(Date.now() + 4 * 3600000).toISOString() },
        { name: "Calculus BC Homework", time: 60, deadline: new Date(Date.now() + 6 * 3600000).toISOString() },
        { name: "Study Psychology Unit 5", time: 50, deadline: new Date(Date.now() + 8 * 3600000).toISOString() },
      ];
      saveTasks(); // Save these sample tasks to local storage
    }
    displayTasks(); // Display the tasks
    suggestTask(); // Update the suggested task
  }

  // ✅ Load tasks when the page loads
  window.onload = loadTasks;
</script>
