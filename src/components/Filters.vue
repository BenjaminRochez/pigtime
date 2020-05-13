<template>
  <div>
    <div>
      <button @click="toggleOrder" class="filter">
          <span v-bind:class="{bold : this.ordering}">Highest priority</span>
          <span style="margin: 0 10px;">/</span>
          <span v-bind:class="{bold : !this.ordering}">Latest</span>
    </button>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
      ordering: true
  }),
  methods: {
      toggleOrder: function(){
          this.ordering = !this.ordering;
          this.$emit('updatedOrder', this.ordering);
      }
  },
};
</script>

<style>

.filter span{
    opacity: .5;
}

span.bold{
    opacity: 1;
}

.filter{
    cursor: pointer;    
    white-space: nowrap;
    display: inline-flex;
    color: inherit;
    background-color: transparent;
    padding: 0.5rem 0.75rem;
    color: #fff;
    font-size: 14px;
}
.filter:focus{
    outline: none;
    border-color: #FA0202;
}
</style>