<template>
  <div>
    <div>
      <button @click="toggleOrder">
          <span v-bind:class="{bold : this.ordering}">Highest priority</span>
          <span v-bind:class="{bold : !this.ordering}"> / Latest</span>
    </button>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
      ordering: false
  }),
  methods: {
      toggleOrder: function(){
          this.ordering = !this.ordering;
          console.log(this.ordering);
          this.$emit('updatedOrder', this.ordering);
      }
  },
};
</script>

<style>
.bold{
    font-weight: 700;
}
</style>