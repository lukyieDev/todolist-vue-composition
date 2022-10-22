<script setup>
import { ref } from "vue";
import { onMounted } from "vue";

const to_dos = ref([]);

const todo = ref("");

const incrementador = ref(1);

const addTodo = () => {
  if (todo.value.length <= 3) {
    alert("not null or much small");
  } else {
    to_dos.value.push({
      id: incrementador.value++,
      text: todo.value,
    });

    localStorage.setItem("to-dos", JSON.stringify(to_dos.value));
  }
};

const removeTodo = ref((array, id) => {
  to_dos.value = array.filter((element) => element.id != id);
  localStorage.setItem("to-dos", JSON.stringify(to_dos.value));
});

onMounted(() => {
  to_dos.value = JSON.parse(localStorage.getItem("to-dos"));
  incrementador.value = to_dos.value[to_dos.value.length - 1].id + 1
});
</script>

<template>
  <div class="main-box">
    <h1 class="title">Todo List</h1>

    <form @submit.prevent="addTodo()" class="todoForm">
      <input
        placeholder="digite sua tarefa"
        v-model="todo"
        class="outline-none mr-5 mb-6"
        type="text"
      />
      <button class="bg-zinc-300 p-1 rounded-sm">Adicionar Tarefa</button>

      <ul>
        <li
          v-for="todo in to_dos"
          :key="todo"
          class="bg-zinc-300 w-fit p-2 rounded-sm my-3"
        >
          {{ todo.text }}

          <span
            @click="removeTodo(to_dos, todo.id)"
            class="bg-zinc-300 w-fit p-2 rounded-sm my-3 cursor-pointer"
            >X</span
          >
        </li>
      </ul>
    </form>
  </div>
</template>

<style lang="postcss" scoped>
.main-box {
  @apply bg-slate-200 w-900 flex items-center flex-col;
}

.title {
  @apply text-3xl font-poppins mb-8;
}
</style>
