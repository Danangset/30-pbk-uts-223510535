<template>
  <div class="container">
    <div class="todo-app">

      <h1>kebiasaan-ku! <br> kebiasaan yang harus dilakukan</h1>
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
const muted = ref(false);

const volume = ref(1);
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
  background: rgb(255, 174, 174);
  width: 100%;
  min-height: 100vh;
  padding: 15px;
  margin-top: -15px;
  margin-left: -15px;
  margin-bottom: -15px;
}

.todo-app {
  width: 100%;
  max-width: 700px;
  background: 3#000000;
  margin: 0 auto 25px;
  padding: 15px 30x 70px;
}
.todo-app h1{
  text-align: right;
  color: #000000;
  margin-bottom: 25px;
}

.row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #080808e7;
  padding-left: 15px;
  border-radius: 15px;
}

input {
  flex: 1;
  border: none;
  outline: none;
  background: transparent;
  padding: 10px;
  font-size: 20px;
  font-weight: 400px;
  color: #ffffff;
}

.row button {
  outline: none;
  padding: 15px 15px;
  background: #ffffff;
  color: #000000;
  font-size: 20px;
  cursor: pointer;
}














.row button:hover{
  background: #000000;
}

ul li {
  list-style: none;
  font-size: 40px;
  padding: 15px 10px 5px 50px;
  user-select: none;
  cursor: pointer;
  position: relative;
  color: #000000;
}

ul li::before {
  content: '';
  position: absolute;
  height: 25px;
  width: 25px;
  border-radius: 15%;
  background-image: url(./assets/unchecked.png);
  background-size: cover;
  background-position: center;
  top: 28px;
  left: 2px;
}

ul li.checked {
  color: rgb(0, 0, 0);
  text-decoration: line-through;
}

ul li.checked::before {
  background-image: url(assets/checked.png);
}

ul li span {
  border-radius: 10px;
  position: absolute;
  right: 0;
  top: 30px;
  width: 35px;
  height: 35px;
  font-size: 35px;
  color: #000000;
  line-height: 25px;
  text-align: center;
}

ul li span:hover {
  background: #000000;
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
  background-color: #fdb7b7;
  color: #fdc5c5;
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
  background-color: #ffd6d6;
}

@media (min-width: 768px) {
  .button-74 {
    min-width: 120px;
    padding: 0 25px;
  }
}
</style>