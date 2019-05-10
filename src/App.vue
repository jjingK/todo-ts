<template>
  <div id="app">
    <todo-header></todo-header>
    <todo-input v-on:addItem="addOneItem"></todo-input>
    <todo-list v-bind:propsdata="todoItems" v-on:removeItem="removeOneItem" v-on:toggleItem="toggleOneItem"></todo-list>
    <todo-footer v-on:clearAll="clearAllItems"></todo-footer>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Emit } from "vue-property-decorator";
import TodoHeader from "@/components/TodoHeader.vue";
import TodoInput from "@/components/TodoInput.vue";
import TodoList from "@/components/TodoList.vue";
import TodoFooter from "@/components/TodoFooter.vue";
import { TodoItem } from '@/types';

@Component({
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  }
})
export default class App extends Vue {
  todoItems: TodoItem[] = [];

  @Emit('addItem')
  addOneItem(item: string): void {
    const obj = {completed: false, item};
    localStorage.setItem(item, JSON.stringify(obj));
    this.todoItems.push(obj);
  }

  @Emit('removeItem')
  removeOneItem(todoItem: TodoItem, index: number) {
    this.todoItems.splice(index, 1);
    localStorage.removeItem(todoItem.item);
  }

  @Emit('toggleItem')
  toggleOneItem(todoItem: TodoItem, index: number) {
    todoItem.completed = !todoItem.completed;
    localStorage.removeItem(todoItem.item);
    localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
  }

  @Emit('clearAll')
  clearAllItems(): void {
    localStorage.clear();
    this.todoItems = [];
  }

  created() {
    if (localStorage.length > 0) {
      for (let i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
          const key = localStorage.key(i) || '';
          const item = JSON.parse(localStorage.getItem(key) || '{}');
          this.todoItems.push(item);
        }
      } 
    }
  }
}
</script>

<style>
body {
  text-align: center;
  background-color: #F6F6F8;
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}
.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
}
</style>
