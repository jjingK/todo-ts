<template>
    <div class="inputBox shadow">
        <input type="text" v-model="newTodoItem" @keyup.enter="addTodo">
        <span class="addContainer" @click="addTodo">
            <i class="addBtn fas fa-plus"></i>
        </span>
    </div>
</template>

<script lang="ts">
import {Component, Vue} from 'vue-property-decorator';

@Component
export default class TodoInput extends Vue {
    newTodoItem: String = '';

    addTodo(): void {
        if (this.newTodoItem !== '') {
            const item = this.newTodoItem.trim();
            this.$emit('addItem', item);
            this.clearInput();
        }
    };
    clearInput(): void {
        this.newTodoItem = '';
    }
}
</script>

<style scoped>
input:focus {
  outline: none;
}
input {
    width: 91%;
    height: 30px;
}
.inputBox {
  background: white;
  height: 50px;
  line-height: 50px;
  border-radius: 5px;
}
.inputBox input {
  border-style: none;
  font-size: 0.9rem;
}
.addContainer {
  float: right;
  background: linear-gradient(to right, #6478FB, #8763FB);
  display: block;
  width: 3rem;
  border-radius: 0 5px 5px 0;
}
.addBtn {
  color: white;
  vertical-align: middle;
}
.closeModalBtn {
  color: #42b983;
}
</style>

