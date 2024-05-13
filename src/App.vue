<script setup>
import { ref, onMounted, computed, watch } from "vue";

// The inputs
const todos = ref([]);
const name = ref("");

const input_content = ref("");

// showing data in an ascending manner
const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

// local storage'
watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});

// Add Todo
const addTodo = () => {
  if (input_content.value.trim() === "") {
    return;
  }
  todos.value.push({
    content: input_content.value,
    done: false,
    createdAt: new Date().getTime(),
  });

  input_content.value = "";
};

// remove funtion
const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

// Toogle function

// Toggle function for each todo
const toggleTask = (todo) => {
  todo.done = !todo.done;
};
</script>

<template>
  <main class="app">
    <section class="container">
      <p class="logo">.</p>

      <h1 class="tap">Todo</h1>

      <section class="create-todo">
        <form @submit.prevent="addTodo">
          <input
            class="enter-task"
            type="text"
            placeholder="start typing..."
            v-model="input_content"
          />
          <input class="btn" type="submit" value="Create" />
        </form>
      </section>
      <section class="todo-list">
        <div class="list">
          <div
            v-for="todo in todos_asc"
            :class="`todo-item ${todo.done && 'done'}
        `"
          >
            <div>
              <button class="task-update" @click="() => toggleTask(todo)">
                {{ todo.done ? "+" : "-" }}
              </button>
            </div>

            <div class="todo-content">
              <input class="item" type="" v-model="todo.content" />
            </div>

            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">x</button>
            </div>
          </div>
        </div>
      </section>
      <div class="botom"></div>
    </section>
  </main>
</template>
<style scoped>
body {
  background: #f1eff1;
}
.container {
  width: 80%;
  height: 280px;

  background-image: url(https://images.pexels.com/photos/417173/pexels-photo-417173.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1);
  background-position: center;

  background-size: contain;
  margin: 40px auto;
  border-radius: 15px;
}

.logo {
  color: rgb(180, 180, 255);
}

.tap {
  font-family: sans-serif;
  font-size: 60px;
  font-weight: 800;
  color: #fff;
  margin-top: 20px;
  margin-left: 50px;
  opacity: 0.8;
}

.create-todo {
  width: 80%;
  margin: auto;
  background: #fcfcfc;
  padding: 5px;
  display: flex;
  justify-content: center;
  border-radius: 20px;
}

form {
  width: 80%;
}

.enter-task {
  width: 80%;
  outline: none;
  padding: 18px 30px;
  border: none;
  border-radius: 18px;
  font-size: 15px;
  position: relative;
  font-weight: 600;
  opacity: 0.9;
}

.btn {
  padding: 12px 17px;
  border: none;
  background: #b95db9;
  color: aliceblue;
  font-family: sans-serif;
  font-size: 15px;
  font-weight: 600;
  border-radius: 15px;
  margin-top: 5px;
}
.todo-list {
  width: 80%;
  margin: 5px auto;
}
.list {
  margin: auto;
  background: rgb(240, 240, 239);
  padding: 5px;
  border-radius: 10px;
  width: 90%;
}

.todo-item {
  display: flex;
  align-items: center;
  background-color: #fff;
  border-bottom: 1px solid black;
}
.todo-content {
  width: 100%;
}
.item {
  padding: 10px;
  border: none;
  font-family: sans-serif;
  font-size: 15px;
  font-weight: 600;
  opacity: 0.5;
  outline: none;
}
.item:hover {
  background: #e7e6e6;
}
.delete {
  font-weight: 600;
  border: none;
  background: #fff;
  color: red;
  font-weight: 600;
  font-size: 15px;
}
.task-update {
  border: none;
  background: #fff;
  margin-right: 5px;
  margin-left: 2px;
  color: #361036;
  font-weight: 600;
  font-size: 15px;
}
</style>
