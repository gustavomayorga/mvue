<style scoped>
.flex{
    display: flex;
}
.squad {
    margin-left: 10px;
    height: 15px;
    width: 15px;
    border: solid 1px white;
}   
</style>
<template>
    <div>
        
        <div class="flex">
        
            <div v-for="x in lifesQtd" :key="x" >
                <div :style="`${qtdVidas[x-1]}`" class="squad" >
                    
                </div>
            </div>
            
        </div>
    </div>
</template>

<script>
export default {
    name: "LifesVue",
    props: {
        lifesQtd: {
            type: Number,
            default: () => 3
        },
        errou: {
            type: Boolean,
            default: () => false
        },
    },
    data() { 
        return{
            qtdVidas: [],
            maxLifes: null,
            
        }
    },
    watch: {
        errou(){
            this.diminuirVida()
        },
        maxLifes(){
            console.log(this.maxLifes)
          
        }
        
    },
    methods: {
        diminuirVida() {
            
            this.qtdVidas[this.maxLifes] = "background: none"
            this.maxLifes--
            
        }
        
    },
    mounted(){
        this.maxLifes = this.lifesQtd - 1
        
        for (let i = 0; i < this.lifesQtd; i++) {
                this.qtdVidas.push(`background: white;`
                )
            }
            
    }
}

</script>