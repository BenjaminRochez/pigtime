<template>
  <li class="todo_item" :class="{'is-focused': this.edit}">
    <Slider  v-on:clicked="onClickChild" :priorit="prior"/> 
    <div class="todo_value" v-if="!this.edit">
      <button @click="toggleTodo">{{todo}}</button>
    </div>
    <div class="todo_form" v-if="this.edit">
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
import Slider from "../components/Slider";
export default {
  components: {
    Slider
  },
  props: {
    value: String,
    id: String,
    editable: Boolean,
    priority: Number
  },
  data: () => ({
    todo: null,
    old_todo: null,
    status: null,
    edit: false,
    identifier: null,
    prior: null
  }),

  methods: {
    onClickChild (value) {
      db.collection('todos')
      .doc(this.identifier)
      .update({
        priority: value
      })
      .then(() =>{
        console.log('success dot');
        this.prior = value;
        this.$emit('dotUpdated', value, this.identifier);
      })
    },
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
        if(this.edit){
            this.$nextTick(() => this.$refs.search.focus());
            this.edit = !this.edit;
        }else{
            this.edit = !this.edit;
        }
    }
  },
  created() {
    this.todo = this.value;
    this.old_todo = this.todo;
    this.identifier = this.id;
    this.prior = this.priority;
    if(this.editable){
      this.edit = true;
      console.log('editable');
      this.$nextTick(() => this.$refs.search.focus());
    }
  }
};
</script>

<style>


.todo_focus{
    height: 100%;
    width: 100%;
    position: fixed;
    top: 0px;
    left: 0px;
    background: rgba(0, 0, 0, .5);
    z-index: 10;
    display: none;
    content: '';
}

.todo_item{
    z-index: 9;
    display: flex;
    -webkit-box-align: center;
    align-items: center;
    position: relative;
    width: 100%;
    
}

.todo_item.is-focused{
  z-index: 11;
  background: red;
}

*, :after, :before {
    box-sizing: border-box;
}
</style>