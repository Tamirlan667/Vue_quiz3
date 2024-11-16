<template>
  <div class="todo-app">
    <!-- Input Section -->
    <div class="input-section">
      <input
        v-model="state.todoInput"
        type="text"
        placeholder="What needs to be done?"
      />
      <button @click="addTodo">Add Task</button>
    </div>

    <!-- Filter Section -->
    <div class="filter-section">
      <button
        @click="state.filter = 'all'"
        :class="{ active: state.filter === 'all' }"
      >
        All
      </button>
      <button
        @click="state.filter = 'completed'"
        :class="{ active: state.filter === 'completed' }"
      >
        Completed
      </button>
      <button
        @click="state.filter = 'incomplete'"
        :class="{ active: state.filter === 'incomplete' }"
      >
        Incomplete
      </button>
    </div>

    <!-- Todo List -->
    <transition-group name="fade" tag="ul">
      <li
        v-for="todo in filteredTodos"
        :key="todo.id"
        :class="{ completed: todo.completed }"
        class="todo-item"
      >
        <input
          type="checkbox"
          @change="toggleCompletion(todo.id)"
          class="todo-checkbox"
          :checked="todo.completed"
        />
        <span>{{ todo.text }}</span>
        <button class="delete-btn" @click="deleteTodo(todo.id)">🗑️</button>
      </li>
    </transition-group>
  </div>
</template>
<script lang="ts">
import { defineComponent, computed } from "vue";
import { useTodoMixin } from "@/mixins/ToDoMixin";

export default defineComponent({
  setup() {
    const { state, addTodo, deleteTodo, toggleCompletion } = useTodoMixin();

    // Computed property for filtered todos based on the selected filter
    const filteredTodos = computed(() => {
      if (state.filter === "completed") {
        return state.todos.filter((todo) => todo.completed);
      } else if (state.filter === "incomplete") {
        return state.todos.filter((todo) => !todo.completed);
      }
      return state.todos; // Show all todos by default
    });

    return {
      state,
      addTodo,
      deleteTodo,
      toggleCompletion,
      filteredTodos,
    };
  },
});
</script>
<style scoped>
/* General App Layout */
.todo-app {
  width: 1000px; /* Set the width to 100% to occupy the full screen */
  height: 50vh; /* Full viewport height */
  margin: 0;
  margin-left: 100px;
  padding: 30px;
  background: linear-gradient(145deg, #ffffff, #f0f0f0);
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  overflow-y: auto;
}

.input-section {
  display: flex;
  justify-content: space-between;
  width: 100%; /* Make sure the input section stretches across the full width */
  max-width: 1200px; /* Optional: You can set a max-width for large screens */
  margin-bottom: 20px;
}

input {
  flex-grow: 1; /* Makes the input field take available space */
  padding: 15px;
  font-size: 18px;
  border-radius: 8px;
  border: 1px solid #ccc;
  outline: none;
  transition: border-color 0.3s ease;
}

input:focus {
  border-color: #28a745;
}

button {
  padding: 15px 30px;
  font-size: 18px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s ease;
}

button:hover {
  background-color: #218838;
}

button:active {
  transform: scale(0.98);
}

/* Filter Section */
.filter-section {
  display: flex;
  justify-content: space-between;
  width: 100%;
  max-width: 1200px; /* Optional: You can set a max-width for large screens */
  margin: 20px 0;
}

.filter-section button {
  padding: 12px 25px;
  font-size: 16px;
  background-color: transparent;
  color: #007bff;
  border: 1px solid #007bff;
  border-radius: 5px;
  transition: background-color 0.3s, color 0.3s;
}

.filter-section button.active {
  background-color: #007bff;
  color: white;
}

.filter-section button:hover {
  background-color: #f1f1f1;
}

/* Todo Item Styling */
ul {
  list-style: none;
  padding: 0;
  width: 100%;
  max-width: 1200px; /* Optional: Set a max-width to control layout */
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 15px;
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s, transform 0.3s ease;
}

.todo-item:hover {
  transform: translateY(-3px);
  background-color: #f9f9f9;
}

.todo-item.completed {
  text-decoration: line-through;
  color: #aaa;
  opacity: 0.7;
}

/* Checkbox Styling */
.todo-checkbox {
  margin-right: 20px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s ease;
}

.todo-checkbox:checked {
  background-color: #28a745;
  border-color: #28a745;
}

/* Delete Button */
.delete-btn {
  background: transparent;
  border: none;
  color: #ff0000;
  cursor: pointer;
  font-size: 22px; /* Increased size for a more visible delete button */
  transition: color 0.3s;
}

.delete-btn:hover {
  color: #d00000;
}

/* Fade Transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
