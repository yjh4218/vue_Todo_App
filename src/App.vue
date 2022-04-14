<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodoItem="addOneItem"></TodoInput>
    <TodoList
      v-bind:propsdata="todoItems"
      v-on:removeItem="removeOneItem"
      v-on:toggleItem="toggleOneItem"
    ></TodoList>
    <TodoFooter v-on:clearAll="clearAllItems"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader";
import TodoInput from "./components/TodoInput";
import TodoList from "./components/TodoList";
import TodoFooter from "./components/TodoFooter";

export default {
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter,
  },
  data() {
    return {
      todoItems: [],
    };
  },
  methods: {
    addOneItem(todoItem) {
      //completed는 체크되었는지 확인용(boolean).
      const obj = { completed: false, item: todoItem };
      //저장하는 로직. JSON.stringify(obj)는 객체를 스트링으로 변환
      localStorage.setItem(todoItem, JSON.stringify(obj));
      this.todoItems.push(obj);
    },
    removeOneItem(todoItem, index) {
      console.log(todoItem, index);
      // db 데이터 삭제
      localStorage.removeItem(todoItem.item);
      // 화면상의 데이터 삭제.
      this.todoItems.splice(index, 1);
    },
    toggleOneItem(todoItem, index) {
      // 클릭할때마다 completed 상태값 변경
      // todoItem.completed = !todoItem.completed;
      // props로 list에 데이터 내린 후, 토글 이벤트 발생 시 이벤트로 인해서 데이터를 직접적으로 수정하는 것은 좋지 않다.
      // 인자값으로 받은 데이터를 수정하는 것보다, 현재 저장되어 있는 데이터를 직접적으로 수정하는게 좋다.
      this.todoItems[index].completed = !this.todoItems[index].completed;
      // localStorage update 기능이 없기 때문에 상태값 갱신을 위해서 삭제 후 다시 추가
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItems() {
      localStorage.clear();
      this.todoItems = [];
    },
  },
  created() {
    //created는 인스턴스가 생성되자마자 실행되는 훅이다. 훅은 생성되는 시점에 해당하는 로직을 실행한다.
    // console.log("created");
    // db 데이터만큼 반복해서 데이터를 넣음.
    if (localStorage.length > 0) {
      for (let i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i) !== "loglevel:webpack-dev-server") {
          // 데이터 값이 string type이기 때문에 다시 객체 형태로 변환한다.
          // 그 후 데이터를 저장한다.
          this.todoItems.push(
            JSON.parse(localStorage.getItem(localStorage.key(i)))
          );
        }
      }
    }
  },
};
</script>

<style>
body {
  text-align: center;
  background-color: #f6f6f6;
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}
.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
