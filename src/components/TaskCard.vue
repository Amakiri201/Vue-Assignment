<script setup lang="ts">
import { ref, type PropType } from "vue";
import { Icon } from "@iconify/vue";
import { useFocus } from "@vueuse/core";

const props = defineProps({
  id: String as PropType<string>,
  task: String as PropType<string>,
  index: Number as PropType<number>,
  editing: Boolean as PropType<boolean>,
  completed: Boolean as PropType<boolean>,
  created: {
    type: Number as PropType<number>,
    required: true,
  },
});

const emits = defineEmits(["edit", "save", "completed", "delete"]);

const editedInput = ref(props.task);

function date(timestamp: number) {
  const date = new Date(timestamp);

  const formattedDate = `${("0" + date.getHours()).slice(-2)}:${(
    "0" + date.getMinutes()
  ).slice(-2)} ${getDayOfWeek(date)} (${getMonthAbbreviation(
    date
  )} ${date.getDate()})`;

  function getMonthAbbreviation(date) {
    const months = [
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
      "Jun",
      "Jul",
      "Aug",
      "Sep",
      "Oct",
      "Nov",
      "Dec",
    ];
    return months[date.getMonth()];
  }

  function getDayOfWeek(date: Date) {
    const daysOfWeek = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
    return daysOfWeek[date.getDay()];
  }

  return formattedDate;
}
</script>

<template>
  <div class="card">
    <div class="checkbox">
      <button
        v-if="!props.completed"
        class="done"
        @click="$emit('completed', index)"
      >
        <Icon icon="line-md:circle" />
      </button>

      <button v-else class="done" @click="$emit('completed', index)">
        <Icon icon="line-md:confirm-circle" />
      </button>

      <div class="date-wrapper">
        <p
          v-if="!props.editing"
          :class="{
            completed: props.completed,
          }"
        >
          {{ props.task }}
        </p>
        <input
          autofocus
          type="text"
          v-else
          v-model="editedInput"
          class="Input-design"
        />
        <h2>{{ date(props.created) }}</h2>
      </div>
    </div>

    <div class="Btn-holder">
      <button
        class="Edit"
        v-if="!props.editing"
        @click="$emit('edit', props.index)"
      >
        <Icon icon="line-md:edit-twotone" />
      </button>
      <button
        v-else
        class="Edit"
        @click="$emit('save', props.index, editedInput)"
      >
        <Icon icon="line-md:confirm" />
      </button>
      <button
        v-if="!props.editing"
        class="Delete"
        @click="$emit('delete', props.id)"
      >
        <Icon icon="line-md:buy-me-a-coffee-filled" />
      </button>
      <button v-else class="Delete" @click="emits('edit', props.index)">
        <Icon icon="line-md:cancel" />
      </button>
    </div>
  </div>
</template>

<style scoped>
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

.card {
  width: 100%;
  display: flex;
  padding: 20px;
  text-align: left;
  color: #273b52;
  margin-bottom: 15px;
  align-items: center;
  background-color: #b1f3de;
  box-shadow: 3px 3px #216e6d;
  justify-content: space-between;
}

.card p {
  font-size: 15px;
  font-weight: bold;
  margin-bottom: 10px;
}

.Edit,
.Delete {
  cursor: pointer;
  border: none;
  display: flex;
  color: #273b52;
  font-size: 25px;
  border-radius: 10px;
  align-items: center;
  justify-content: center;
  background: transparent;
}

.Btn-holder {
  gap: 10px;
  display: flex;
  align-items: center;
}

.checkbox {
  gap: 10px;
  display: flex;
  width: 100%;
  align-items: center;
}
.done {
  border: none;
  font-size: 30px;
  cursor: pointer;
  color: #1a2f20;
  background-color: transparent;
}

.completed {
  text-decoration: line-through;
}

.Input-design {
  width: 100%;
  border: none;
  opacity: 0.5;
  outline: none;
  font-size: 15px;
  font-weight: bold;
  background: transparent;
  border-bottom: 1px solid #1a2f20;
}

.date-wrapper {
  width: 100%;
  margin-right: 15px;
}
</style>
