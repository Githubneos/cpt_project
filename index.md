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

  <ul id="taskList"></ul>
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
    width: 400px;
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

  ul {
    margin-top: 20px;
    text-align: left;
  }

  li {
    background: #f0f0f0;
    margin: 5px 0;
    padding: 8px;
    border-radius: 6px;
  }
</style>

<script>
  // ✅ LIST used to manage all task data and help sort, suggest, display tasks
  let tasks = [];

  // ✅ PROCEDURE: Gets input and sends data as parameters
  function handleNewTask() {
    const name = document.getElementById("taskName").value;
    const time = parseInt(document.getElementById("timeNeeded").value);
    const deadline = new Date(document.getElementById("deadline").value);

    if (!name || isNaN(time) || isNaN(deadline.getTime())) return;

    // ✅ Parameters passed to addTask
    addTask(name, time, deadline);
  }

  // ✅ PROCEDURE
  // ✅ Parameters: name (string), time (int), deadline (Date)
  // ✅ Return Type: void
  // ✅ Sequencing: push → sort → display
  function addTask(name, time, deadline) {
    const task = { name, time, deadline };

    // ✅ COLLECTION (list): used to store structured task objects
    tasks.push(task);

    // ✅ Sorting tasks using a list to manage complexity
    tasks.sort((a, b) => a.deadline - b.deadline);

    displayTasks();   // ✅ Call to custom procedure
    suggestTask();    // ✅ Call to custom procedure
  }

  // ✅ PROCEDURE: Recommends next task based on list sorting
  // ✅ Uses SELECTION (if length 0), then shows next task
  function suggestTask() {
    if (tasks.length === 0) {
      document.getElementById("suggestedTask").textContent = "You're all caught up!";
      return;
    }

    const top = tasks[0]; // ✅ Top task (most urgent)
    document.getElementById("suggestedTask").textContent =
      `${top.name} - ${top.time} mins - Due: ${top.deadline.toLocaleString()}`;
  }

  // ✅ PROCEDURE to display list of tasks
  // ✅ ITERATION: loops through the task list to output UI
  function displayTasks() {
    const list = document.getElementById("taskList");
    list.innerHTML = "";

    tasks.forEach((task, index) => {
      const li = document.createElement("li");
      li.textContent = `${task.name} - ${task.time} mins - Due: ${task.deadline.toLocaleString()}`;

      // ✅ Done button to remove task (list management)
      const doneBtn = document.createElement("button");
      doneBtn.textContent = "✅ Done";
      doneBtn.onclick = () => {
        tasks.splice(index, 1);  // ✅ Remove from list
        displayTasks();
        suggestTask();
      };

      doneBtn.style.marginLeft = "10px";
      doneBtn.style.background = "#f44336";
      doneBtn.style.color = "white";
      doneBtn.style.border = "none";
      doneBtn.style.padding = "5px 10px";
      doneBtn.style.borderRadius = "6px";
      li.appendChild(doneBtn);
      list.appendChild(li);
    });
  }
</script>
