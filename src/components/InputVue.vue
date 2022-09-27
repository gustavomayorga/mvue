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
   /* .input:focus{
        border: white 3px solid;
    } */
</style>
<template>    
    <input class="input" v-model="valor" type="number" />
</template>
<script>
export default {
    name: "InputVue",
    valor: null,
    
}
</script>