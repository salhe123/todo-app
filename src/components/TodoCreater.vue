<script setup>
import { reactive,defineEmits } from "vue";
import TodoButton from "./TodoButton.vue";
const emit=defineEmits([
    "create-todo"
]);
const check=reactive({
    todo:"",
    invalid:null,
    errMsg:"",

});
const createTodo=()=>{
check.invalid=null;
if (check.todo !=="") {
    emit("create-todo",check.todo);
    check.todo="";
    return;
    
}
check.invalid=true;
check.errMsg="todo value can not be empty";
};

</script>

<template>
  <div class="input-wrap" :class="{'input-err':check.invalid}">
    <input type="text" v-model="check.todo" />
   <TodoButton @click="createTodo()" />
  </div>
  <p v-if="check.invalid" class="err-msg">{{check.errMsg}}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }

  
}
.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>