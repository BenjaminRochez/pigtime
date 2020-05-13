<template>
  <div class="home" v-on:keydown.alt="create">
    <img alt="Vue logo" src="../assets/logo.png" />
    <button @click="create">Create</button>
    <ul>
      <Todo
        v-for="item in orderedTodo"
        :key="item.id"
        :value="item.value"
        :id="item.id"
        :editable="item.edit"
        :priority="item.priority"
        v-on:dotUpdated="updateDot"
      />
      <div class="todo_focus"></div>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src
import _ from "lodash";
import Todo from "@/components/Todo.vue";
import db from "@/firebase/init";
export default {
  name: "Home",
  components: {
    Todo
  },
  data: () => ({
    items: []
  }),

  methods: {
    create: function() {
      db.collection("todos")
        .add({
          value: "New task",
          priority: 5
        })
        .then(res => {
          let item = {
            value: "New task",
            priority: 5
          };
          item.id = res.id;
          item.edit = true;
          this.items.push(item);
          console.log(item);
        });
    },
    updateDot: function(value, identifier) {
      var myindex = this.findWithAttr(this.items, 'id', identifier);
      this.items[myindex].priority = value;
    },
    findWithAttr: function(array, attr, value) {
      for (var i = 0; i < array.length; i += 1) {
        if (array[i][attr] === value) {
          return i;
        }
      }
      return -1;
    }
  },

  computed: {
    orderedTodo: function() {
      return _.orderBy(this.items, "priority");
    }
  },

  created() {
    db.collection("todos")
      .get()
      .then(snapshot => {
        snapshot.forEach(doc => {
          let item = doc.data();
          item.id = doc.id;
          this.items.push(item);
          console.log(this.items);
        });
      })
      .catch(err => {
        console.log(err);
      });
  },
  mounted() {
    window.addEventListener("keydown", e => {
      var key = e.which || e.keyCode;

      if (e.altKey && key === 78) {
        this.create();
      }
    });
  }
};
</script>