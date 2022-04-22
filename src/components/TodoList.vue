<template>
  <div>
    <!-- name은 css class와 관련있다. css 그룹의 리스트 아이템 트랜지션 효과 참고-->
    <!-- tag는  -->
    <transition-group name="list" tag="ul">
      <!-- v-for 반복문 사용시 데이터 저장하는 공간과 index를 설정할 수 있다. 이로 인해 지정한 데이터를 삭제할 수 있다. -->
      <li
        v-for="(todoItem, index) in this.storedTodoItems"
        v-bind:key="todoItem.item"
        class="shadow"
      >
        <span class="checkBtn">
          <i
            class="fa-solid fa-check"
            v-bind:class="{ checkBtnCompleted: todoItem.completed }"
            v-on:click="toggleComplete({ todoItem, index })"
          ></i>
        </span>
        <!-- todoItem의 completed의 값에 따라 체크 실행됨(줄 긋기) 즉, 조건에 따라 실행될 수 있게 함-->
        <span v-bind:class="{ textCompleted: todoItem.completed }">
          {{ todoItem.item }}
        </span>
        <span class="removeBtn" v-on:click="removeTodo({ todoItem, index })">
          <i class="fa-solid fa-trash-can"></i>
        </span>
      </li>
    </transition-group>
  </div>
</template>

<script>
import { mapGetters, mapMutations } from "vuex";

export default {
  methods: {
    ...mapMutations({
      removeTodo: "removeOneItem",
      toggleComplete: "toggleOneItem",
    }),
    // removeTodo(todoItem, index) {
    //   // this.$emit("removeItem", todoItem, index);
    //   this.$store.commit('removeOneItem', {todoItem, index});
    // },

    // toggleComplete(todoItem, index) {
    //   // this.$emit("toggleItem", todoItem, index);
    //   this.$store.commit("toggleOneItem", { todoItem, index });
    // },
  },
  computed: {
    // todoItems(){
    //   return this.$store.getters.storedTodoItems
    // }
    ...mapGetters(["storedTodoItems"]),
  },
};
</script>

<style scoped>
ul {
  list-style-type: none;
  padding-left: 0px;
  margin-top: 0;
  text-align: left;
}
li {
  display: flex;
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: white;
  border-radius: 5px;
}
.checkBtn {
  line-height: 45px;
  color: #62acde;
  margin-right: 5px;
}
.checkBtnCompleted {
  color: #b3adad;
}
.textCompleted {
  text-decoration: line-through;
  color: #b3adad;
}
.removeBtn {
  margin-left: auto;
  color: #de4343;
}

/* 리스트 아이템 트랜지션 효과 */
/* 리스트 효과가 나타나는 시간 */
.list-enter-active,
.list-leave-active {
  transition: all 1s;
}
/* 리스트의 효과. 즉 실행하는 동안 보여짐. */
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>
