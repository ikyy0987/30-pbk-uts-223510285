<template>
    <div class="container">
      <div class="todo-app">

        <h1>Kegiatan-Ku! <br> Hal yang harus dilakukan</h1>
        <div class="row">
          <input type="text" v-model="newTask" placeholder="Ketik kegiatan">
          <button @click="addTask">ADD</button>
        </div>

        <div class="filters">
          <button @click="hideCompleted = !hideCompleted" class="button-74">

            {{ hideCompleted ? 'tampil' : 'sembunyi' }}
          </button>
        </div>




        <ul>
          <li v-for="(task, index) in filteredTodos" :key="task.id" :class="{ checked: task.checked }" @click="toggleTask(task)">
            {{ task.text }}
            <span @click.stop="removeTask(task)">&#xd7;</span>
            <span @click.stop="editTask(task)">✎</span>
          </li>
        </ul>
      </div>


    </div>

  </template>
  
  <script setup>
import { ref, computed } from 'vue';

const newTask = ref('');
const tasks = ref([]);
const hideCompleted = ref(false);

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? tasks.value.filter((task) => !task.checked)
    : tasks.value;
});

function addTask() {
  if (newTask.value.trim() === '') {
    alert("Isi terlebih dahulu!");
  } else {
    tasks.value.unshift({ id: Date.now(), text: newTask.value, checked: false });
    newTask.value = '';
    saveData();
  }
}

function toggleTask(task) {
  task.checked = !task.checked;
  saveData();
}

function removeTask(task) {
  const index = tasks.value.findIndex((t) => t.id === task.id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
    saveData();
  }
}

function editTask(task) {
  const newText = prompt("Edit task:", task.text);
  if (newText !== null) {
    task.text = newText;
    saveData();
  }
}

function saveData() {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}

function loadData() {
  const savedTasks = localStorage.getItem("tasks");
  tasks.value = savedTasks ? JSON.parse(savedTasks) : [];
}

loadData();
</script>


  
  <style scoped>
  .container {
    background: rgb(61, 61, 61);
    width: 100%;
    min-height: 100vh;
    padding: 10px;
    margin-top: -10px;
    margin-left: -10px;
    margin-bottom: -10px;
  }
  
  .todo-app {
    width: 100%;
    max-width: 800px;
    background: #727272;
    margin: 0 auto 30px;
    padding: 10px 30px 70px;
  }
  .todo-app h1{
    text-align: right;
    color: #000000;
    margin-bottom: 20px;
  }
  
  .row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #ffffffe7;
    padding-left: 10px;
    border-radius: 10px;
  }
  
  input {
    flex: 1;
    border: none;
    outline: none;
    background: transparent;
    padding: 10px;
    font-size: 18px;
    font-weight: 400px;
    color: #000000;
  }
  
  .row button {
    outline: none;
    padding: 10px 10px;
    background: #9b9b9b;
    color: #fff;
    font-size: 18px;
    cursor: pointer;
  }














  .row button:hover{
    background: #424242;
  }
  
  ul li {
    list-style: none;
    font-size: 40px;
    padding: 10px 8px 2px 45px;
    user-select: none;
    cursor: pointer;
    position: relative;
    color: #000000;
  }
  
  ul li::before {
    content: '';
    position: absolute;
    height: 30px;
    width: 30px;
    border-radius: 10%;
    background-image: url(./assets/unchecked.png);
    background-size: cover;
    background-position: center;
    top: 28px;
    left: 2px;
  }
  
  ul li.checked {
    color: rgb(65, 65, 65);
    text-decoration: line-through;
  }
  
  ul li.checked::before {
    background-image: url(assets/checked.png);
  }
  
  ul li span {
    border-radius: 10px;
    position: absolute;
    right: 0;
    top: 28px;
    width: 30px;
    height: 30px;
    font-size: 30px;
    color: #9b9b9b;
    line-height: 26px;
    text-align: center;
  }
  
  ul li span:hover {
    background: #424242;
  }
  
  .filters{
  display: flex;
  justify-content: center;
  padding-top: 2vh;
    
  }
  
  .filters button{
    width: 20px;
  }
  
  
  .button-74 {
    background-color: #9b9b9b;
    color: #ffffff;
    cursor: pointer;
    display: inline-block;
    font-weight: 200;
    font-size: 14px;
    padding: 0 14px;
    line-height: 25px;
    text-align: center;
    text-decoration: none;
    user-select: none;
    -webkit-user-select: none;
    touch-action: manipulation;
  }
  
  .button-74:hover {
    background-color: #424242;
  }
  
  @media (min-width: 768px) {
    .button-74 {
      min-width: 120px;
      padding: 0 25px;
    }
  }
  </style>
