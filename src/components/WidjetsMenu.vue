<template>
    <div class="menu">
        <button @click="addText">Текст</button>
        <button>Картинка</button>
        <button>Фигура</button>
    </div>
</template>

<script>
    export default{
        name: 'WidjetsMenu',
       
        methods: {
           addText(){
            this.$emit('addText')
           }
        }
    }
</script>

<style scoped>
    .menu{
        border: 2px solid black;
        padding: 15px;
        width: 10%;
    }

    button{
        height: 50px;
        margin-right: 10px;
    }
</style>