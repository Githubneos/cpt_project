---
layout: base
title: Football
permalink: /shared_interests/football/
menu: nav/shared_interests.html
---

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

  <input id="taskName" placeholder="Task Name" />
  <input id="timeNeeded" placeholder="Estimated Time (min)" type="number" />
  <input id="deadline" type="datetime-local" />
  <button onclick="handleNewTask()">Add Task</button>

  <h3>🧠 Suggested Task:</h3>
  <p id="suggestedTask">No tasks yet.</p>

  <h3>📅 Task List</h3>
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
  // list of tasks
  let tasks = [];

  // procedures
  function handleNewTask() {
    const name = document.getElementById("taskName").value;
    const time = parseInt(document.getElementById("timeNeeded").value);
    const deadline = new Date(document.getElementById("deadline").value);
    if (!name || isNaN(time) || isNaN(deadline.getTime())) return;
    addTask(name, time, deadline);
  }

  function addTask(name, time, deadline) {
    const task = { name, time, deadline: deadline.toISOString() };
    tasks.push(task);
    tasks.sort((a, b) => new Date(a.deadline) - new Date(b.deadline));
    saveTasks();
    displayTasks();
    suggestTask();
  }

  function suggestTask() {
    if (tasks.length === 0) {
      document.getElementById("suggestedTask").textContent = "You're all caught up!";
      return;
    }
    const top = tasks[0];
    document.getElementById("suggestedTask").textContent =
      `${top.name} - ${top.time} mins - Due: ${new Date(top.deadline).toLocaleString()}`;
  }

  function displayTasks() {
    const table = document.querySelector("#taskTable tbody");
    table.innerHTML = "";
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

  function removeTask(index) {
    tasks.splice(index, 1);
    saveTasks();
    displayTasks();
    suggestTask();
  }

  function saveTasks() {
    localStorage.setItem("studyTasks", JSON.stringify(tasks));
  }

  function loadTasks() {
    const saved = localStorage.getItem("studyTasks");
    if (saved) {
      tasks = JSON.parse(saved);
      displayTasks();
      suggestTask();
    }
  }

  window.onload = loadTasks;
</script>
