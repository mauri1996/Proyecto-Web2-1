<template>
    <img class="imm" :src="url"/>
</template>

<script>
export default {
    name: 'Imagen',
    props:{
        url:{
            type: String
        }
    },
    data(){
        return{

        }
    }
}
</script>

<style>
.imm{
    height: 100px;
    width: 100px;
}
</style>