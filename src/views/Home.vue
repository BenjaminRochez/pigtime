<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <button @click="create">Create</button>
    <Todo v-for="item in items" :key="item.id" :value="item.value" :id="item.id" />
  </div>
</template>

<script>
// @ is an alias to /src
import Todo from '@/components/Todo.vue'
import db from '@/firebase/init'
export default {
  name: 'Home',
  components: {
    Todo
  },
  data: () => ({
    items: [],
  }),

  methods: {
    create: function(){
      db.collection('todos')
      .add({
        value: 'New task'
      })
      .then(res =>{
        console.log(res.id);
          let item = {
            value: 'New task'
          };
          item.id = res.id;
          this.items.push(item);
          console.log(this.items);
      })
    }
  },

  created(){
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
  }
  
}
</script>
