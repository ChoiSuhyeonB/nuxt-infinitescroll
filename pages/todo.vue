<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput @addTodo="addTodo"></TodoInput>
    <TodoList @removeTodo="removeTodo"></TodoList>
    <TodoFooter @removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script lang="ts">
import TodoHeader from "~/components/TodoHeader.vue";
import TodoInput from "~/components/TodoInput.vue";
import TodoList from "~/components/TodoList.vue";
import TodoFooter from "~/components/TodoFooter.vue";
import { Component, State, Vue } from "nuxt-property-decorator";
import AxiosInstance from "~/util/api";
import { createNamespacedHelpers } from "vuex";

import { param } from "~/api/todolist";
import axios from "axios";

@Component({
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  }
})
export default class MainComponent extends Vue {
  //변수
  public newTodoItem: String = "";
  public deleteTodoItem: String = "";

  //생성될때 실행되는 메서드
  private created() {
    this.fetchData();
  }

  public async init() {
    // await this.$axios.get('/api/todolist').then((res)=>{
    //   //this.$store.commit('items/changeTodoItems', res.data)
    //  this.todoItems= res.data
    // })
    /*
      await AxiosInstance.getSearch().then((res)=>{
        this.$store.commit('items/changeTodoItems', res.data)
       this.todoItems= res.data
      })
      */
  }

  public async addTodo(todoItem: string) {
    this.newTodoItem = todoItem;
    await AxiosInstance.postAdd(this.newTodoItem).then(res => {
      console.log("add data : " + this.newTodoItem);
    });
  }

  public async clearAll() {
    //  localStorage.clear();
    //  this.todoItems = [];
    await AxiosInstance.deleteClearAll().then(res => {});
  }
  public async removeTodo(todoItem: string, index: number) {
    //  localStorage.removeItem(todoItem);
    //  this.todoItems.splice(index, 1)
    this.deleteTodoItem = todoItem;
    await AxiosInstance.deleteClear(this.deleteTodoItem).then(res => {});
  }

  // infinite scroll 구현

  //computed
  get url() {
    return "/api/todolist/" + this.$store.state.page;
  }
  async fetchData() {
    const res = await this.$axios.get(this.url);
    this.$store.commit("changeTodoItems", res.data);
  }
}
</script>

<style>
body {
  text-align: center;
  background-color: #91e0fd;
}
ul {
  list-style: none;
  text-align: left;
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}
.shadow {
  box-shadow: 10px 10px 10px rgba(0, 0, 0, 0.03);
}
.shadow {
  box-shadow: 10px 10px 10px rgba(0, 0, 0, 0.03);
}
button {
  border-style: groove;
}
</style>
