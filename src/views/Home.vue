<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <Todo v-for="item in items" :key="item.id" :value="item.value"/>
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
