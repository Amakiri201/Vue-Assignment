<script setup>
import { ref } from "vue";
import { Icon } from "@iconify/vue";
import { v4 as uuid } from "uuid";
import TaskCard from "./components/TaskCard.vue";

const tasks = ref([]);
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
  <div class="blob blob-top-right" />
  <div class="blob blob-bottom-left" />
  <div class="blob blob-bottom-right" />

  <div class="wrapper">
    <div class="hero">
      <h1>GOJO</h1>
      <p>A simplest to-do app </p>
    </div>

    <form @submit.prevent="handleSubmit">
      <label for="Input" class="label-text">Enter a task</label>
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

    <div>
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
  z-index: 2;
  width: 100%;
  padding: 0 20px;
  position: relative;
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
  font-family: "GraphitePro-Bold";
  font-size: 50px;
  color: #ffd388;
  margin-bottom: 20px;
}

p {
  font-size: 15px;
  font-family: "GraphitePro-Medium";
}

.label-text {
  display: inline-block;
  margin-bottom: 8px;
  margin-left: 10px;
  color: #1e304a;
  font-size: 15px;
  font-weight: 600;
}

.card {
  width: 100%;
  display: flex;
  padding: 0 0px;
  height: 55px;
  text-align: left;
  color: #273b52;
  margin-bottom: 15px;
  align-items: center;
  border-radius: 15px;
  background-color: #b1f3de;
  box-shadow: 3px 3px #216e6d;
  justify-content: space-between;
}

.Add {
  width: 40px;
  min-width: 40px;
  max-width: 40px;
  height: 40px;
  border: none;
  display: flex;
  font-weight: bold;
  font-size: 20px;
  padding: none;
  cursor: pointer;
  color: #273b52;
  border-radius: 50%;
  align-items: center;
  background: transparent;
  justify-content: center;
  background-color: #fdd287;
  box-shadow: 3px 3px #216e6d;
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
  height: 45px;
  outline: none;
  font-size: 12px;
  font-weight: bold;
  padding: 0px 16px;
  outline-width: 1px;
  line-height: normal;
  border-radius: 15px;
  letter-spacing: 1px;
  box-shadow: 3px 3px #216e6d;
  font-family: "GraphitePro-Medium";
  border: 1px solid rgb(219, 219, 219);
}

@font-face {
  font-family: "GraphitePro-Bold";
  src: local("GraphitePro-Bold"), url("./assets/fonts/GraphitePro-Bold.otf"),
    format("truetype");
  font-weight: bold;
}
@font-face {
  font-family: "GraphitePro-Medium";
  src: local("GraphitePro-Medium"), url("./assets/fonts/GraphitePro-Medium.otf"),
    format("truetype");
  font-weight: Medium;
}
@font-face {
  font-family: "GraphitePro-ExtraLight";
  src: local("GraphitePro-ExtraLight"),
    url("./assets/fonts/GraphitePro-ExtraLight.otf"), format("truetype");
  font-weight: Light;
}

@media (min-width: 650px) {
  h1 {
    font-size: 100px;
  }
}
</style>
