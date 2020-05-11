<template>
  <li>
    <div class="todo_dot"></div>
    <div class="todo_value">
      <button @click="toggleTodo">{{todo}}</button>
    </div>
    <div class="todo_form">
      <form action="#">
        <input type="text" :value="todo" />
        <button @click.prevent="sendData">Done</button>
      </form>
    </div>
  </li>
</template>

<script>
import db from "@/firebase/init";
export default {
  props: {
    value: String
  },
  data: () => ({
    todo: null
  }),

  methods: {
    sendData: function() {
      db.collection("todos")
        .add({
          value: this.todo
        })
        .then(res => {
          console.log(res);
        })
        .catch(err => {
          console.log("Error", err);
        });
    }
  },
  created(){
      this.todo = this.value
  }
};
</script>