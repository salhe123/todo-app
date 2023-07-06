<script setup>
import TodoCreater from '../components/TodoCreater.vue'
import TodoItem from '../components/TodoItem.vue'
import { Icon } from '@iconify/vue';
import { ref,watch } from "vue"
import { uid } from "uid"
const todoList = ref([

]);
watch(todoList,(newvalue,oldvalue)=>{
  
})
const fetchTodolist=()=>{
const savedTodosList=JSON.parse(localStorage.getItem("todolist"))
if(savedTodosList){
  todoList.value=savedTodosList;
}
};
fetchTodolist();
const setTodoListLocalStorage=()=>{
localStorage.setItem("todolist",JSON.stringify(todoList.value))
};
const creatTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    iscompleted: null,
    isEditing: null,


  });
  setTodoListLocalStorage();
};
const toggleTodoComplete=(todopos)=>{
todoList.value[todopos].iscompleted =! todoList.value[todopos].iscompleted
setTodoListLocalStorage();
};
const toggleEdittodo=(todopos)=>{
  todoList.value[todopos].isEditing =! todoList.value[todopos].isEditing
  setTodoListLocalStorage();
};
const updatetodo=(todoval,todopos)=>{
  todoList.value[todopos].todo = todoval;
  setTodoListLocalStorage();

};
const deletetodo=(todId)=>{
todoList.value = todoList.value.filter((todo)=>todo.id!== todId)
setTodoListLocalStorage();
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreater @create-todo="creatTodo" />
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem v-for="(todo, index) in todoList" :todo="todo"
       :index="index" 
       @toggle-complete="toggleTodoComplete" @edit-todo="toggleEdittodo" 
       @update-todo="updatetodo"
       @delete-todo="deletetodo"
       />
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one</span>
    </p>
  </main>
</template>



<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>