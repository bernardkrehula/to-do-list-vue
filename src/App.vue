<script setup lang="ts">
import { computed, ref } from "vue";

const todos = ref<
  { content: string; category: string; done: boolean; createdAt: number }[]
>([]);

const input_content = ref("");
const input_category = ref<string | null>(null);

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  }),
);

const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null)
    return;
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });
};
</script>

<template>
  <main>
    <section class="create-todo">
      <h3>Create a todo</h3>
      <form @submit.prevent="addTodo">
        <input
          type="text"
          placeholder="e.g make a video"
          v-model="input_content"
        />
        <h4>Pick a category</h4>
        <div class="options">
          <label
            ><input
              type="radio"
              name="category"
              value="business"
              v-model="input_category"
            />
            <span class="bubble buisness"></span>
            <div>Buisness</div>
          </label>
          <label
            ><input
              type="radio"
              name="category"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add todo" />
      </form>
    </section>
    <section class="todo-list">
      <div class="list">
        <div
          v-for="todo in todos_asc"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
