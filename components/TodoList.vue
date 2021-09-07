<template>
  <section>
    <ul v-if="$store.state.items">
      <li v-for="(todoItem, index) in $store.state.items" :key="index">
        <i class="checkBtn fas fa-check" aria-hidden="true"></i>
        {{ todoItem.name }}
        <span
          class="removeBtn"
          type="Button"
          @click="removeTodo(todoItem.id, index)"
        >
          <i class="far fa-trash-alt" aria-hidden="true"></i>
        </span>
      </li>
    </ul>
    <infinite-loading
      spinner="spiral"
      @infinite="infiniteScroll"
    ></infinite-loading>

     <!-- <RecycleScroller
      class="scroller"
      page-mode
      v-if="$store.state.items"
      :items="this.$store.state.items"
      :item-size="52"
      v-slot="{ item }"
    >
      <div class="user">
        {{ item.name }}
      </div>
    </RecycleScroller> -->
  </section>
</template>

<script lang="ts">
import { Component, Vue, Prop, Emit } from "nuxt-property-decorator";

// @ts-ignore
import VueVirtualScroller from "vue-virtual-scroller";
@Component
export default class TodoList extends Vue {
  //props:['propsdata']

  //@Prop() propsdata: string[] | undefined;
  //@Prop() page: any | undefined;
  //임시 배열 변수 Data입니다.
  Data: string[] = [""];
  //computed
  get url() {
    return "/api/todolist/" + this.$store.state.page;
  }
  public async infiniteScroll($state: {
    loaded: () => void;
    complete: () => void;
  }) {
    setTimeout(async => {
      //this.page = this.page + 10;
      this.$store.commit("changePage", this.$store.state.page + 10);
      this.Data = [];
      this.$axios
        .get(this.url)
        .then(res => {
          if (res && res.data.length > 1) {
            res.data.forEach((todoItem: any) => this.Data.push(todoItem));
            this.Data.forEach((todoItem: any) =>
              this.$store.commit("pushTodoItems", todoItem)
            );

            $state.loaded();
          } else {
            $state.complete();
          }
        })
        .catch(err => {
          console.log(err);
        });
    }, 1000);
  }

  public removeTodo(todoItem: string, index: number): void {
    this.$emit("removeTodo", todoItem, index);
  }
}
</script>

<style scoped>
ul {
  /* 리스트에 점을 없애준다. */
  list-style-type: none;
  padding-left: 0px;
  margin-top: 0;
  text-align: left;
  margin-left: 10px;
}
li {
  display: flex;
  min-height: 30px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: white;
  border-radius: 20px;
}
.checkBtn {
  line-height: 45px;
  color: rgb(7, 177, 126);
  margin-left: 30px;
  margin-right: 70px;
}
.removeBtn {
  margin-left: auto;
  color: rgb(245, 9, 213);
}
</style>
