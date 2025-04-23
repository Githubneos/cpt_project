---
layout: post
title: Taskmanager 
search_exclude: true
description: Manage you tasks
hide: true
menu: nav/home.html
---

<div class="container">
  <h2>Task Manager</h2>
  <input id="taskInput" type="text" placeholder="Enter a new task" />
  <button onclick="addTask()">Add Task</button>

  <!-- ✅ List of tasks -->
  <ul id="taskList"></ul>
</div>

<style>
  /* ✅ Cool animated background */
  body {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(120deg, #89f7fe, #66a6ff);
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }

  .container {
    background: rgba(255, 255, 255, 0.1);
    border-radius: 20px;
    padding: 40px;
    box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
    backdrop-filter: blur(10px);
    width: 400px;
    color: #fff;
    text-align: center;
  }

  input, button {
    padding: 10px;
    border-radius: 10px;
    border: none;
    margin: 10px 0;
    width: 100%;
  }

  ul {
    text-align: left;
    margin-top: 20px;
  }

  li {
    background: rgba(255, 255, 255, 0.2);
    margin: 5px 0;
    padding: 8px;
    border-radius: 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  li.completed {
    text-decoration: line-through;
    background: rgba(0, 255, 0, 0.1);
  }

  .task-buttons button {
    background: rgba(255, 0, 0, 0.5);
    color: white;
    border: none;
    padding: 5px;
    border-radius: 5px;
    cursor: pointer;
    transition: 0.3s;
  }

  .task-buttons button:hover {
    background: rgba(255, 0, 0, 0.8);
  }
</style>

<script>
  // ✅ List to store tasks
  let tasks = [];

  // ✅ Student-developed procedure
  function addTask() {
    const taskInput = document.getElementById("taskInput");
    const taskText = taskInput.value.trim();
    if (!taskText) return;

    // Add the task to the tasks list
    tasks.push({ text: taskText, completed: false });

    // Update the task list UI
    updateTaskList();
    
    // Clear the input field
    taskInput.value = "";
  }

  // ✅ Procedure to update the task list UI
  function updateTaskList() {
    const taskList = document.getElementById("taskList");
    taskList.innerHTML = ''; // Clear the list

    tasks.forEach((task, index) => {
      // Create a list item for each task
      const taskItem = document.createElement("li");
      if (task.completed) taskItem.classList.add("completed");

      // Add the task text
      const taskText = document.createElement("span");
      taskText.textContent = task.text;
      taskItem.appendChild(taskText);

      // Create buttons for completing/removing tasks
      const buttons = document.createElement("div");
      buttons.classList.add("task-buttons");

      // ✅ Complete button
      const completeButton = document.createElement("button");
      completeButton.textContent = task.completed ? "Undo" : "Complete";
      completeButton.onclick = () => toggleComplete(index);
      buttons.appendChild(completeButton);

      // ✅ Remove button
      const removeButton = document.createElement("button");
      removeButton.textContent = "Remove";
      removeButton.onclick = () => removeTask(index);
      buttons.appendChild(removeButton);

      taskItem.appendChild(buttons);
      taskList.appendChild(taskItem);
    });
  }

  // ✅ Procedure to mark task as completed/undo completion
  function toggleComplete(index) {
    tasks[index].completed = !tasks[index].completed;
    updateTaskList();
  }

  // ✅ Procedure to remove task from list
  function removeTask(index) {
    tasks.splice(index, 1); // Remove task from the array
    updateTaskList(); // Update the UI
  }
</script>

