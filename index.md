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
  let tasks = [];

  // ✅ INPUT procedure: gets input and uses parameters
  function handleNewTask() {
    const name = document.getElementById("taskName").value;
    const time = parseInt(document.getElementById("timeNeeded").value);
    const deadline = new Date(document.getElementById("deadline").value);

    if (!name || isNaN(time) || isNaN(deadline.getTime())) return;

    // ✅ Calling with parameters
    addTask(name, time, deadline);
  }

  // ✅ PROCEDURE with parameters & sequencing
  // ✅ Parameters: name (string), time (int), deadline (Date)
  // ✅ Return type: void
  function addTask(name, time, deadline) {
    const task = { name, time, deadline: deadline.toISOString() };

    // ✅ Add to collection
    tasks.push(task);

    // ✅ Sequencing: push → sort → store → display
    tasks.sort((a, b) => new Date(a.deadline) - new Date(b.deadline));
    saveTasks(); // ✅ Save to local storage

    displayTasks();   // ✅ Procedure call
    suggestTask();    // ✅ Procedure call
  }

  // ✅ PROCEDURE: visual and textual output
  function suggestTask() {
    if (tasks.length === 0) {
      document.getElementById("suggestedTask").textContent = "You're all caught up!";
      return;
    }

    const top = tasks[0];
    document.getElementById("suggestedTask").textContent =
      `${top.name} - ${top.time} mins - Due: ${new Date(top.deadline).toLocaleString()}`;
  }

  // ✅ PROCEDURE using ITERATION to display table
  function displayTasks() {
    const table = document.querySelector("#taskTable tbody");
    table.innerHTML = "";

    // ✅ Loop through collection
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

  // ✅ PROCEDURE to delete task from collection
  function removeTask(index) {
    tasks.splice(index, 1);       // ✅ Selection & modification
    saveTasks();                  // ✅ Save change
    displayTasks();               // ✅ Re-render
    suggestTask();                // ✅ Recalculate suggestion
  }

  // ✅ PROCEDURE: saves tasks
  function saveTasks() {
    localStorage.setItem("studyTasks", JSON.stringify(tasks));
  }

  // ✅ PROCEDURE: loads tasks (device input)
  function loadTasks() {
    const saved = localStorage.getItem("studyTasks");
    if (saved) {
      tasks = JSON.parse(saved);
      displayTasks();
      suggestTask();
    }
  }

  // ✅ Load tasks at startup (device input)
  window.onload = loadTasks;
</script>
