<template>
  <div>
    <div class="todo__dot__container" @click="toggleDot" v-if="!toggled">
      <div
        class="todo__dot"
        v-bind:style="{height: 'calc(' + dotSize + 'rem / 3.5)', width: 'calc(' + dotSize + 'rem / 3.5)', borderRadius: 'calc(' + dotSize + 'rem / 3.5)'}"
      ></div>
    </div>
    <form action v-if="toggled">
      <div class="slider__container">
        <input
          type="range"
          min="1"
          max="10"
          value="4"
          class="slider"
          id="myRange"
          v-model="dotSize"
        />
      </div>
      <button class="todo__dot__btn" @click.prevent="toggleDot">Done</button>
    </form>
  </div>
</template>
<script>
export default {
  data: () => ({
    dotSize: null,
    toggled: false
  }),
  props: {
    priorit: String
},
  methods: {
    toggleDot: function() {
      if(this.toggled){
          this.$emit('clicked', this.dotSize);
          this.toggled = !this.toggled;
      }else{
          this.toggled = !this.toggled;
      }
    }
  },
  created() {
      if(this.priorit){
          console.log('ici');
          this.dotSize = this.priorit;
      }else{
          this.dotSize = '5'
      }
    }
};
</script>

<style>
.slider {
  -webkit-appearance: none;
  width: 100%;
  height: 22px;
  cursor: pointer;
  border-radius: 0px;
  background: none;
  outline: none;
}
.slider__container::before {
  content: "";
  position: absolute;
  width: calc(100% - 18px);
  background-image: linear-gradient(
    90deg,
    red,
    red 90%,
    transparent 90%,
    transparent 100%
  );
  background-size: 11.1111% 1px;
  top: 0px;
  bottom: 0px;
  left: 0px;
  right: 0px;
  height: 2px;
  z-index: -1;
  margin: auto;
}

.slider__form {
  display: flex;
  min-height: 22px;
  -webkit-box-align: center;
  align-items: center;
  -webkit-box-pack: justify;
  justify-content: space-between;
}

.slider__container {
  width: 100%;
  max-width: 30rem;
  margin-right: 1rem;
  position: relative;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 25px;
  background: red;
  border: 3px solid #fff;
  cursor: pointer;
  border-radius: 25px;
  box-shadow: 0px 0px 1px 1px red;
}

.todo__dot {
  content: "";
  background-color: #e03800;
  
  border-radius: 100%;
  display: inline-block;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate3d(-50%, -50%, 0);
}

.todo__dot__btn {
  cursor: pointer;
  white-space: nowrap;
  display: inline-flex;
  color: white;
  background-color: #e03800;
  font: inherit inherit inherit inherit inherit inherit inherit inherit inherit;
  padding: 0.5rem 0.75rem;
  border-width: 1px;
  border-style: solid;
  border-color: transparent;
  border-image: initial;
  
}

.todo__dot__container{
    min-width: calc(10rem / 3.5);
    min-height: calc(10rem / 3.5);
    text-align: center;
    position: relative;
    vertical-align: middle;
    cursor: pointer;
}
</style>