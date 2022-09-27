<style>
.full-line{
    height: 5px;
    border-radius: 10px;
    background-color: rgb(139, 0, 0);
    
    
}
.progress-line{
    border-radius: 10px;
    height: 5px;
    
}
.right{
    position: relative;
    left: 99%;
}


</style>
<template>
    <div>
        <div class="line">
            <b class="right"  >{{ ~~count }}</b>
            <div class="full-line" >
                <div class="progress-line" :style="`background-color: #E64348; width: ${!gameOver ? width : 0.001}%; transition: ${!gameOver ? timerAnimation : 0}s `" >

                </div>
            </div>
            
        </div>

    </div>
</template>
<script>
export default {
    name: "TimeLine",
    data() {
        return {
            count: 0,
            timerAnimation: 0,
            countdown: false,

            width: 100,
            pointBonus: false,
            
        }
    },
    props: {
        acertar: {
            type: Boolean,
            default: () => false 
        },
        totalTimer: {
            type: Number, 
            default: () => 15
        },
        startBol: {
            type: Boolean,
            default: () => false
        },
        getTempo: {
            type: Function,
            default: () => 0
        },
        gameOver: {
            type: Boolean,
            default: () => false
        }
    },  
    watch: {
        // Utilizar o cleartimeout para remover o loop
        // reset 
        gameOver() {
            if(this.gameOver){
                    this.count = 0
                    clearTimeout(this.countdown)
                    return 0

            }
        },
        acertar(){
            if(this.gameOver){
                return 0
            }
            this.count = this.totalTimer
            
            this.resetarBarra();
            clearTimeout(this.countdown)
        },

        startBol() {
            this.count = this.totalTimer
            this.timerAnimation = this.totalTimer
            this.width = 0
        },
        count() {
            if(this.gameOver){
                return 0
            }
            if(this.count > 0 && this.startBol){
                
                this.cronometro() 
            }  
        },
        
    },
   
    methods: {
        
        dataTime(){
            this.$emit('getTempo', ~~this.count)
        },
        cronometro() {
            this.countdown = setTimeout(() => {
                    this.count -= 0.1
                    this.dataTime()
                    this.timerAnimation = this.totalTimer
                    this.width = 0
                }, 100);
            
        },
        resetarBarra(){
        
            this.timerAnimation = 0
            this.width = 100
           
        }
    }
}
</script>