<template>
  <div>
    <button :class="button">
      
      {{ calculo }}
    </button>
  </div>
</template>

<script>
export default {
  name: "SelectableButton",
  props: {
    num: {
      type: Number,
    },
    calculo: {
      type: String,
      default: () => {
        return "A + B = C";
      },
    }, 
    selected: { /* PROP BOOLEAN PARA O STYLE */
      type: Number,
      default: () => {
        return 2;
      },
    },
  },
  data() {
    return {
      
    };
  },
  computed: {
      button () { /* VARIAVEL SELECTED DO BUTTON MUDANDO O STYLE */
          if(this.selected === this.num){
              return 'real'
          } else {
              return 'the'
          }
      }
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Arvo&family=Jockey+One&display=swap");
.real {
  width: 105px;
  border-radius: 45px;
  background: #ff494f;
  color: white;
  font-family: "Jockey One", sans-serif;
  font-size: 20px;
  padding-top: 5px;
  
  margin-top: 13px;
  padding-bottom: 5px;
  box-shadow: 0px 1px 0px 1px rgba(0, 0, 0, 0.3);
  border: #ffffff 2px solid;
  
  cursor: pointer;
  transition: 0.2s;
}

.real:active {
  border: 1px solid #ffffff;
}
.the {
  width: 100px;
  border-radius: 45px;
  background: #ff4950;
  color: white;
  font-family: "Jockey One", sans-serif;
  font-size: 20px;
  padding-top: 5px;
  margin-top: 15px;
  padding-bottom: 5px;
  box-shadow: 0px 1px 0px 1px rgba(0, 0, 0, 0.3);
  border: #ff4950 1px solid;
  cursor: pointer;
  transition: 0.2s;
}
.the:hover {
  border: 1px solid rgb(255, 255, 255);
}
.the:active {
  border: 1px solid rgb(255, 255, 255);
}
</style>
