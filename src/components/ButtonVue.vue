<template>
  <div>
    <button class="vue-button">
      {{ texto }}
    
    </button>
  </div>
</template>

<script>
export default {
  name: "SelectableButton",
  props: {
    height: {
      type: Number,
      default: () => null
    },
    select: {
      type: Number,
      default: () => 0
    },
    texto: {
        type: String,
        default: () => 'Button'
    }
  },
  data() {
    return {
      
    };
  },
  mounted() {
    if(this.height !== null) this.heightStyle
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Arvo&family=Jockey+One&display=swap");

.vue-button {
  width: 150px;
  border-radius: 5px;
  background: #ff4950;
  color: white;
  font-family: "Jockey One", sans-serif;
  font-size: 25px;
  padding: 5px;
  margin-top: 25px;
  height: 60px;
  box-shadow: 0px 1px 0px 1px rgba(0, 0, 0, 0.3);
  border: #ff4950 1px solid;
  cursor: pointer;
  transition: 0.2s;
}
.vue-button:hover {
  border: 1px solid rgb(255, 255, 255);
}
.vue-button:active {
    background: #ff3f46;
  margin-top: 27px;
}
</style>
