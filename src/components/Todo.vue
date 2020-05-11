<template>
  <li>
    <div class="todo_dot"></div>
    <div class="todo_value" v-if="!this.shown">
      <button @click="toggleTodo">{{todo}}</button>
    </div>
    <div class="todo_form" v-if="this.shown">
      <form action="#">
        <input type="text" ref="search" v-model="todo" />
        <button @click.prevent="sendData">Done</button>
      </form>
    </div>
    <button @click="deleteTodo">Remove</button>
  </li>
</template>

<script>
import db from "@/firebase/init";
export default {
  props: {
    value: String,
    id: String
  },
  data: () => ({
    todo: null,
    old_todo: null,
    status: null,
    shown: false,
    identifier: null
  }),

  methods: {
    sendData: function() {
      if (this.todo != this.old_todo) {
        db.collection("todos")
          .doc(this.identifier)
          .update({
            value: this.todo
          })
          .then(() => {
              console.log('updating');
            this.toggleTodo();
            this.old_todo = this.todo;
          })
          .catch(err => {
            console.log("Error", err);
          });
      } else {
        this.toggleTodo();
      }
    },
    deleteTodo: function(){
        db.collection('todos')
        .doc(this.identifier)
        .delete()
        .then(() =>{
            this.$destroy();

      // remove the element from the DOM
      this.$el.parentNode.removeChild(this.$el);
        })
    },
    toggleTodo: function() {
        if(this.shown){
            this.$nextTick(() => this.$refs.search.focus());
            this.shown = !this.shown;
        }else{
            this.shown = !this.shown;
        }
    }
  },
  created() {
    this.todo = this.value;
    this.old_todo = this.todo;
    this.identifier = this.id;
  }
};
</script>