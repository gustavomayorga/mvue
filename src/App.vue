<template>
  <div>
    <div class="body-app"  v-if="f1[0]" :style="`opacity: ${opacity2}; transition: 0.1 `">
      <h1>MVUE</h1>
      <div class="levels">
          <h2>Selecíone um nível</h2>
          <div class="flex" >
            <div class="item"  v-for="(item,index) in fases" :key="index">
              <SelectableButton  :calculo="item.equa" :num="index" :selected="numSelected" @click="clicked(index)"/>
            </div>
          </div>
      </div>
      
      <div class="button">
          <ButtonVue style="margin-right:20px" texto="Singleplayer" :select="numSelected"  @click="singleplayer()" />
          <ButtonVue style="margin-left:20px" texto="Multiplayer" :select="numSelected" />
      </div>
    </div>
    <div class="body-app" v-if="f1[1]">

    </div>
    
    <SinglePlayer v-if="f1[2]" :equac="numSelected" :style="`display: block ;opacity: ${opacity}; transition: 0.2s`" @back="retornar" />
    
  </div>
</template>

<script>
import SelectableButton from './components/SelectableButton.vue';
import ButtonVue from './components/ButtonVue.vue';
import SinglePlayer from './components/SinglePlayer.vue';
export default {
    name: "App",
    components: { SelectableButton, ButtonVue, SinglePlayer },
    data() {
      return {
        numSelected: null,
        opacity: 0.0, 
        opacity2: 1,
        f1:[ true, false , false],
        fases: [
                {equa:"A + B = ?" },{equa:"A - B = ?" },{equa:"A +- B = ?" },{equa:"A +- ? = B" },{equa:"A + B + C = ?" },
                {equa:"A + B - C = ?" },{equa:"A +- B +- ? = C" },{equa:"A * B = ?" },{equa:"A * ? = B" },{equa:"A / B = ?" },
                {equa:"A / ? = B" },{equa:"A */ B = ?" },{equa:"A² = ?" },{equa:"A * B +- C = ?" },{equa:"Random" }
              ]
      }
    },
    watch: {
      
    },  
    methods:{
      retornar(){
        this.f1[0] = true
        this.f1[2] = false
        this.opacity = 0
        this.opacity2 = 1
      },
      clicked(id){
        this.numSelected = id
      },
      selectNv(){
        return this.numSelected
      },
      singleplayer(){
        if(this.numSelected === null){
          

          this.numSelected = this.fases.length - 1
          this.f1[0] = false
          this.f1[2] = true
          
        } else {
          
          this.f1[0] = false
          this.f1[2] = true
          
        }
        // clearTimeout
        setTimeout( () => { this.opacity = 1
          this.opacity2 = 0 } , 0.1)
        }
      
    }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Arvo&family=Jockey+One&display=swap');

.button{
  display: flex;
  justify-content: center;
}

.flex{
  display: flex;
  flex-flow: row wrap;
  padding-left: 20px;
  padding-right: 20px;
  justify-content: space-between;
  
}
.item{
  display: contents;
  
}

.item:nth-child(5n)::after {
  content: '';
  
  width: 100%;
}
.levels{
  
  width: 79%;
  height: 60%;
  background: #802528;
  border-radius: 10px;
  margin: auto;
}
.body-app {
  width: 800px;
  height: 500px;
  line-height: 50px;
  text-align: center;
  background: #401214;
  border-radius: 10px;
  color: white;
  position: absolute;
  top: 50%;
  margin-top: -250px;
  left: 50%;
  margin-left: -400px;
  font-family: 'Jockey One', sans-serif;
  
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
  
}
</style>
