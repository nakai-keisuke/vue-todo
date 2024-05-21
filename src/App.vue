<template>
  <p>Todo List (Vue)</p>

  <!-- 新規作成欄 -->
  <input v-model="text" />
  <button @click="saveTodo">保存</button>

  <!-- 一覧表示欄 -->
  <ul>
    <li v-for="(todo, index) in todoList" :key="index">
      <button @click="updateTodo">更新</button>
      <button @click="deleteTodo" :id="todo.id">削除</button>
      <input v-model="todo.text" />
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      todoList: [],
      text: "",
      isDisable: true,
    };
  },

  created() {
    if (!localStorage.getItem("todoList")) {
      localStorage.setItem("todoList", "[]");
    }
    this.todoList = JSON.parse(localStorage.getItem("todoList"));
  },

  methods: {
    setTodo(list) {
      localStorage.setItem("todoList", JSON.stringify(list));
      this.todoList = list;
    },

    saveTodo() {
      if (this.todoList.length) {
        this.setTodo([
          ...this.todoList,
          ...[{ id: this.todoList.at(-1).id + 1, text: this.text }],
        ]);
      } else {
        this.setTodo([{ id: 1, text: this.text }]);
      }
      this.text = "";
      this.isDisable = true;
    },

    updateTodo() {
      this.setTodo(this.todoList);
    },

    deleteTodo(e) {
      const newTodoList = this.todoList.filter(
        (todo) => todo.id !== Number(e.target.id)
      );
      this.setTodo(newTodoList);
    },
  },
};
</script>

<style></style>
