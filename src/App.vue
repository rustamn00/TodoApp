<script setup>
import { ref, onMounted, computed, watch } from "vue";
import ExportAppVue from "./components/ExportApp.vue";
import ImportApp from "./components/ImportApp.vue";

const todos = ref([]);
const name = ref("");

const input_content = ref("");

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
  })
);

watch(name, (newVal) => {
  sessionStorage.setItem("name", newVal);
});

watch(
  todos,
  (newVal) => {
    sessionStorage.setItem("todos", JSON.stringify(newVal));
  },
  {
    deep: true,
  }
);

const addTodo = () => {
  if (input_content.value.trim() === "") {
    return;
  }

  todos.value.push({
    content: input_content.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime(),
  });

  if (input_content.value.trim() !== "") {
    input_content.value = "";
  }
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

onMounted(() => {
  name.value = sessionStorage.getItem("name") || "";
  todos.value = JSON.parse(sessionStorage.getItem("todos")) || [];
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2>
        Hello
        <input type="text" id="name" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form id="new-todo-form" @submit.prevent="addTodo">
        <input
          class="todo"
          type="text"
          name="content"
          id="content"
          placeholder="e.g learn vue"
          v-model="input_content"
        />

        <input type="submit" value="Add todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list" id="todo-list">
        <div
          v-for="todo in todos_asc"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span class="bubble dark"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
    <div class="import-export">
      <label>
        <ImportApp />
      </label>

      <label>
        <ExportAppVue />
      </label>
    </div>
  </main>
</template>
