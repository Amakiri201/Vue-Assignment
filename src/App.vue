<script setup>
import { ref } from "vue";
import { Icon } from "@iconify/vue";
import { v4 as uuid } from "uuid";
import TaskCard from "./components/TaskCard.vue";

const tasks = ref([
  {
    id: uuid(),
    editing: false,
    completed: false,
    created: Date.now(),
    task: "test data",
  },
]);
const currentInput = ref("");

function handleDelete(id) {
  tasks.value = tasks.value.filter((task) => task.id != id);
}

function handleEdit(index) {
  tasks.value[index].editing = !tasks.value[index].editing;
}

function handleCompleted(index) {
  tasks.value[index].completed = !tasks.value[index].completed;
}

function handleSave(index, value) {
  tasks.value[index].task = value;
  tasks.value[index].editing = !tasks.value[index].editing;
}

function handleSubmit() {
  const newTask = {
    id: uuid(),
    editing: false,
    completed: false,
    created: Date.now(),
    task: currentInput.value,
  };
  tasks.value.push(newTask);
  currentInput.value = "";

  console.log(tasks.value);
}
</script>

<template>
  <!-- <div>
    <img src="./assets/blob.svg" class="top-right" alt="top background" />
  </div> -->

  <div class="wrapper">
    <div class="hero">
      <h1>GOJO</h1>
      <p>The simplest todo app ever</p>
    </div>

    <form @submit.prevent="handleSubmit">
      <label for="Input" class="input-text">Enter a task</label>
      <section class="hero-footer">
        <input
          class="inputField"
          id="Input"
          type="text"
          autocomplete="off"
          v-model="currentInput"
        />
        <button :disabled="!currentInput" type="submit" class="Add">
          <Icon icon="line-md:plus" />
        </button>
      </section>
    </form>

    <div class="card-list">
      <TaskCard
        v-for="(task, index) in tasks"
        :id="task.id"
        :index="index"
        :task="task.task"
        :created="task.created"
        :editing="task.editing"
        :completed="task.completed"
        @edit="handleEdit"
        @save="handleSave"
        @delete="handleDelete"
        @completed="handleCompleted"
      />
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 100%;
  padding: 0 20px;
}
.hero {
  display: flex;
  margin-top: 150px;
  text-align: center;
  margin-bottom: 20px;
  flex-direction: column;
}

.card h2 {
  font-size: 10px;
  font-weight: 200;
}

h1 {
  font-size: 100px;
}

p {
  font-size: 15px;
}

.input-text {
  display: inline-block;
  margin-bottom: 8px;
  margin-left: 10px;
  color: #ffff;
  font-size: 15px;
  /* color: #1e304a; */
  font-weight: 600;
}

input {
  width: 100%;
  height: 30px;
  outline: none;
  box-shadow: none;
  padding: 0px 16px;
  border-radius: 5px;
  outline-width: 1px;
  border: 1px solid rgb(219, 219, 219);
}
.card {
  width: 100%;
  display: flex;
  padding: 20px;
  text-align: left;
  color: #273b52;
  margin-bottom: 15px;
  align-items: center;
  border-radius: 15px;
  background-color: #b1f3de;
  box-shadow: 3px 3px #216e6d;
  justify-content: space-between;
}

.card p {
  font-size: 15px;
  font-weight: bold;
  margin-bottom: 10px;
}
.card-list {
}

.Add,
.Edit,
.Delete {
  width: 30px;
  height: 30px;
  border: none;
  display: flex;
  color: #273b52;
  font-size: 20px;
  border-radius: 10px;
  align-items: center;
  justify-content: center;
  border: 1px solid #1a2f20;
  background-color: #fdd287;
}

.Btn-holder {
  gap: 10px;
  display: flex;
  align-items: center;
}

.hero-footer {
  gap: 16px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}

.inputField {
  width: 100%;
  height: 30px;
  outline: none;
  box-shadow: none;
  padding: 0px 16px;
  border-radius: 5px;
  outline-width: 1px;
  border: 1px solid rgb(219, 219, 219);
}
</style>
