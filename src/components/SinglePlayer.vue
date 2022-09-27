<style scoped>
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}
    input{
        outline: none;
        border: none
    }
    
    .input{
        background: #802528;
        color: white;
        font-family: 'Jockey One', sans-serif;
        text-align: center;
        font-size: 16pt;
        border-radius: 10px;
        width: 105px;
        height: 60px;
        
    }
.body-singleplayer{
    font-family: 'Jockey One', sans-serif;
    color: white;
}
.top-singleplayer{
    position: absolute;
    width: 100%;
    top: 0;
    left: 0;
    height: 50px;
    background-color: #802528;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;

}
.levels{
    text-align: right;
    margin-right: 20px;
    
}
.score{
    text-align: center;
    
}
.itens{
    margin-top: auto;
    margin-bottom: auto;
    font-size: 20pt;
}
.margin-auto{
    margin: auto;
}
.timer{
    margin-top: 70px;
    width:  80%;
    padding: 10px;
    margin-bottom: 40px;
    
}
.screen{
      background: #401214;
      padding: 35px;
      width: 50%;
      text-align: center;
      border-radius: 10px;
      font-size: 25pt;
      
}
.question-screen{
    background: #FF4950;
    padding: 50px;
    border-radius: 10px;
}
.control{
    
    display: grid; 
    grid-auto-columns: 1fr; 
    grid-template-columns: 1fr 1fr; 
    gap: 0px 0px; 
    margin: 5%;    
    
    justify-items: center;
}
.input-vue{
    justify-self: right;
    margin-top: auto;
    margin-right: 45px;   
}
.button-vue{
    
    margin-top: auto;
    justify-self: left;
    
}
</style>
<template>
   <div class="body-singleplayer">
        <div class="top-singleplayer"> 
            <div class="itens" >
                <LifesVue :errou="errar" />
            </div>
            <div class="score itens" >
                <b :style="`color: red; position: relative; opacity: ${aPontos.opacity}; top: ${aPontos.top}px; transition: 1s;`">+{{ somaScore }}</b> {{score}}
            </div>
            <div class="levels itens" >
                {{`Nível ${level}/10`}}
            </div>  
        </div>  
        <div class="timer margin-auto">
            
            <TimeLine :acertar="resetarCssTimer" :startBol="start" :totalTimer="timer" @getTempo="setTime" :gameOver="gameOver" />
        </div>      
        <div class="screen margin-auto">
                <div class="question-screen" >
                    <h1>
                        {{ question }}
                        
                    </h1>
                    
                </div>
                <div class="control ">
                    <div class="input-vue " >
                        <input class="input" v-model="valor" type="number"  v-on:keyup.enter="responder()" :disabled="!start"  />
                        <!--<InputVue v-model="valor" v-on:keyup.enter="responder()" />-->
                    </div>
                    <div class="button-vue ">
                        <ButtonVue texto="VOLTAR" :style="`display:${gameOver ? 'block' : 'none'}`" @click="retornar()" />
                        <ButtonVue texto="START" :style="`display:${!start ? 'block' : 'none'}`" @click="start = true; resetarQuest = !resetarQuest;" />
                        <ButtonVue texto='ENTER' :style="`display:${start&&!gameOver ? 'block' : 'none'}`" @click="responder()"/>
                    </div>
                </div>
            </div>
        </div>
    
</template>

<script>
import TimeLine from './TimeLine.vue';
import ButtonVue from './ButtonVue.vue';
import LifesVue from './LifesVue.vue';

export default {
    name: "SinglePlayer",
    props: {
        equac: {
            type: Number,
            default: () => 0
        },
        back: {
            type: Function, 
            default: () => false
        }
    },
    data() { 
        return {
            resetarCssTimer: false,
            errar: null,
            perder: false,
            valor: null,
            vidas: 3,
            start: false,
            resetarQuest: false,
            tempo: 0,
            timer: 0,
            somaScore: 0,
            nvSelected: null,
            animarPontos: false,
            random: false,
            maxScore: 200,
            minScore: 100,
            nvMax: false,
            aPontos: {
                top: 35,
                opacity: 0
            },
            
            a: '?',
            b: '?',
            c: '?',
            r: null,
            question: "Clique em START",
            score: 0,
            nv: 10,
            level: 1,
            cleanTimer: null,
            gameOver: false,
        };
    },
    mounted(){
        this.nvSelected = this.equac
        this.timer = this.nvSelected + 10
    },
    watch: {
        vidas() {
            console.log(this.vidas)
            if(this.vidas === 0){
                this.gameOver = true
                this.resetarQuest = !this.resetarQuest
                
            }
        },
        perder(){
            this.cleanTimer = setTimeout(() => {
                this.errar = !this.errar
                this.resetarCssTimer = !this.resetarCssTimer
                this.resetarQuest = !this.resetarQuest
                this.vidas--
            }, this.timer * 1000);
        },
        animarPontos(){
            this.aPontos.top = 0
            this.aPontos.opacity = 1
            setTimeout(() => {
                this.aPontos.opacity = 0
            }, 500);
            
        },
        
        score(){
            if (this.level === 10) {
                this.nvMax = true
                
            }
            if (this.maxScore > this.score && this.score > this.minScore && !this.nvMax) {
                let aux = this.maxScore
                this.maxScore *= 1.8
                this.minScore = aux
                this.level++
                this.nv = 10*this.level
       
                this.timer += this.nvSelected + this.level
            } 
        },

        resetarQuest(){
            if(this.gameOver){
                this.question = " GAME OVER "
                clearTimeout(this.cleanTimer)
                return 0
            }
            if(this.random){
                
                this.nvSelected = Math.floor(Math.random() * 14 )
            }
            let randomizar = Math.random() * 20
            this.a = this.getRandomNum()
            this.b = this.getRandomNum()
            this.c = this.getRandomNum()
            this.perder = !this.perder
            
            let helper = 0
            switch (this.nvSelected) {
                case 0:
                    this.r = this.a + this.b
                    this.question = `${this.a} + ${this.b} = ?`
                    break;
                case 1:
                    this.r = this.a - this.b
                    this.question = `${this.a} - ${this.b} = ?`
                    break;
                case 2:
                    if(randomizar > 10){
                        this.r = this.a + this.b
                        this.question = `${this.a} + ${this.b} = ?`
                        break;
                    } else {
                        this.r = this.a - this.b
                        this.question = `${this.a} - ${this.b} = ?`
                        break;
                    }

                case 3:
                    if(randomizar > 10){
                        
                        this.r = this.a + this.b
                        helper = this.r
                        this.r = this.b
                        this.b = helper

                        this.question = `${this.a} + ? = ${this.b}`
                        break;
                    } else{
                        

                        this.r = this.a - this.b
                        helper = this.r
                        this.r = this.b
                        this.b = helper

                        this.question = `${this.a} -  ? = ${this.b}`
                    break;}
                case 4:
                    this.r = this.a + this.b + this.c 
                    this.question = `${this.a} + ${this.b} + ${this.c} = ? `
                    break;
                case 5:
                    this.r = this.a + this.b - this.c 
                    this.question = `${this.a} + ${this.b} - ${this.c} = ? `
                    break;
                case 6:
                    
                    if(randomizar > 10){
                        if (~~randomizar%2 == 0) {

                            this.r = this.a + this.b + this.c
                            helper = this.r 
                            this.r = this.c 
                            this.c = helper 
                            this.question = `${this.a} + ${this.b} +  ? = ${this.c} `
                            break
                        } else {

                            this.r = this.a + this.b - this.c 
                            helper = this.r 
                            this.r = this.c 
                            this.c = helper
                            this.question = `${this.a} + ${this.b} -  ? = ${this.c} `
                            break    
                        }
                    } else {
                        if (~~randomizar%2 == 0) {
                            this.r = this.a - this.b + this.c
                            helper = this.r 
                            this.r = this.c 
                            this.c = helper
                            this.question = `${this.a} - ${this.b} +  ? = ${this.c} `
                            break
                        } else {
                            this.r = this.a - this.b - this.c 
                            helper = this.r 
                            this.r = this.c 
                            this.c = helper
                            this.question = `${this.a} - ${this.b} -  ? = ${this.c} `
                            break
                        }
                    }
                    
                case 7:
                    this.r = this.a * this.b 
                    this.question = `${this.a} * ${this.b} = ? `
                    break;
                case 8:
                    this.r = this.a * this.b 
                    helper = this.r
                    this.r = this.b 
                    this.b = helper 

                    this.question = `${this.a} * ? = ${this.b}` 


                    break;
                case 9:

                    if(this.verificarDivisao()){
                        this.r = this.a / this.b
                        this.question = `${this.a} / ${this.b} = ?`
                        break
                    }


                    break;
                case 10:
                    if(this.verificarDivisao()){
                        this.r = this.a / this.b 
                        helper = this.r 
                        this.r = this.b
                        this.b = helper
                        this.question = `${this.a} / ? = ${this.b}`
                        break
                    }
                    break;
                case 11:
                    if(randomizar > 10){

                        this.r = this.a * this.b 
                        this.question = `${this.a} * ${this.b} = ?`

                        break;

                    } else {

                        this.verificarDivisao()

                        this.r = this.a / this.b
                        this.question = `${this.a} / ${this.b} = ?`
                        break
                    }

                    
                case 12:
                    this.r = Math.pow(this.a, 2);
                    this.question = `${this.a}² = ?`
                    break;

                case 13:
                    if(randomizar > 10){
                        this.r = this.a*this.b + this.c
                        this.question = `${this.a} * ${this.b} + ${this.c} = ?`

                    } else {
                        this.r = this.a*this.b - this.c
                        this.question= `${this.a} * ${this.b} - ${this.c} = ?`
                    }
                    break;
               
                case 14:
                    this.nvSelected = Math.floor(Math.random() * 14 )
                    this.random = true
                    this.resetarQuest = !this.resetarQuest
                    break;
                default:
                    break;
            }
        },
    },
    methods:{
        retornar(){
            this.$emit('back', true)
        },
        setTime(value){
            
            this.tempo = value
            
        },
        verificarDivisao(){
            while (this.a == 0) {
                this.a = Math.floor(Math.random() * this.nv)
            }
            while (this.b == 0) {
                this.b = Math.floor(Math.random() * this.nv)
            }
            if (this.a%this.b != 0) {
                
                this.a = this.b * Math.floor(Math.random() * this.nv)
                
            }
            return true
        },
        responder(){
            
            setTimeout(() => {
                this.aPontos.top = 35
            }, 1000);
            if(this.valor === this.r){
                this.resetarCssTimer = !this.resetarCssTimer
                this.valor = null
                this.animarPontos = !this.animarPontos
                this.somaScore = 10 + this.level + this.tempo
                this.score += this.somaScore
                this.resetarQuest = !this.resetarQuest
                clearTimeout(this.cleanTimer)
                
            }else {
                this.errar = !this.errar
                this.vidas--
                this.valor = null
                
            }
        },
        getRandomNum(){
            return Math.floor(Math.random() * this.nv )
        },
    
    },
    components: { TimeLine, ButtonVue, LifesVue }
}
</script>