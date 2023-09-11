<script setup>
import {ref, onMounted, watch} from "vue";
import { v4 as uuidv4 } from "uuid";
const todo_lists = ref([]);
const todo_text_input = ref("");
const count = ref(0)


function addTodo(){
    console.log(todo_text_input.value)
    if(todo_text_input.value.trim()!==""){
        let id = uuidv4()
        todo_lists.value.unshift({
                                id: id,
                                todo:todo_text_input.value.trim() ,
                                isFinished:false,
                                });
        console.log(JSON.stringify(todo_lists.value))
        todo_text_input.value = ""
    }
    
}

function deleteTodo(id){
    console.log("===deleteTodo===")
    console.log(id)
    todo_lists.value.filter
    todo_lists.value = todo_lists.value.filter((todo) => todo.id !== id);
    console.log(todo_lists)
}
watch(
    todo_lists,
  (newValue) => {
    localStorage.setItem("todo_lists", JSON.stringify(newValue));
  },
  { deep: true }
);

onMounted(() => {
    todo_lists.value = JSON.parse(localStorage.getItem("todo_lists")) || [];
    console.log("===onMounted===")
    console.log(todo_lists.value)
});
</script>

<template>
    <main class="container mx-auto">
        <header class="m-2">
            <h1 class="text-6xl font-thin select-none">TODO!</h1>
            <div class="font-semibold select-none text-neutral-600">simple and studid todo app</div>
        </header>
        <form class="px-10 py-12 bg-white shadow-sm" @submit.prevent="">
            <section class="flex">
            <input type="text" placeholder="做點重要的事吧..." class="w-full text-2xl focus:outline-none input-lg input input-bordered" v-model="todo_text_input" />
            <button class="text-xl btn-lg btn btn-neutral" @click="addTodo">新增</button>
            </section>
        </form>
        <section class="px-10 py-6 mt-4 bg-white">
            <div>You have {{ todo_lists.length }} item(s) to do</div>
            <!--div v-show="todo_lists.length === 0">You have {{ todo_lists.length }} item(s) to do</div-->
        <ul class="" v-for="todoItem in todo_lists">
          <li class="flex items-center justify-between py-6 border-b">
            <div class="flex items-center gap-3">
              <input type="checkbox" id="todo_id_1" class="checkbox" v-model="todoItem.isFinished" />
              <label for="todo_id_1" class="text-xl cursor-pointer" :class="{ 'line-through':todoItem.isFinished}">
                {{todoItem.todo}}
              </label>
            </div>
            <div>
              <button class="p-2 hover:text-white hover:bg-neutral" @click="deleteTodo(todoItem.id)">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0"
                  />
                </svg>
              </button>
            </div>
          </li>
          </ul>
        </section>
    </main>
</template>

<style scoped></style>
