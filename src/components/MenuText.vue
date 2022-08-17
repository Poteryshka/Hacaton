<template>
    <div class="widjetsMenu">
        <p>Размер шрифта: {{fontSizeElement}}px</p>

        <input type="range" min="1" max="100" value="10"
                @change="changeFontSize($event)"/>

        <div class="palitra">
            <div 
                v-for="color in colorBase"
                @click="changeColorText(color)" 
                :key="color"
                class="palitraElement"
                :id="color"
                :style="{ 'background-color': color}">
            </div>

        </div>
        
        <button @click="deleteElement">Удалить</button>

        
    </div>
</template>

<script>
    export default{
        props: {
            id: Number,
            fontSizeElement: Number,
        },
        methods: {
            changeFontSize(event){
                const val = Number(event.target.value)
                this.$emit('changeFontSize', val, this.id)
            },
            deleteElement(){
                this.$emit('deleteElement', this.id)
            },
            changeColorText(colorText){
                this.$emit('changeColorText', this.id, colorText)
            }
        },
        data(){
            return{
                colorBase: ['#FFFFFF', '#000000', '#808080', '#C0C0C0', '#FF00FF', 
                            '#800080', '#000080', '#FF0000', '#FFFF00', '#808000',
                            '#00FF00', '#008000', '#0000FF', '#FF4500', '#8B0000'],
            }
        }
    }
</script>

<style scoped>
    .widjetsMenu{
        width:15%;
        padding: 10%;
    }

    p{
        font-size: 1em;
        color: black;
    }

    .palitra{
        margin-top: 10%;
        background-color: brown;
        width: 400%;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
        border: 1px solid black;
    }

    .palitraElement{
        /* border: 1px solid black; */
        padding: 50%; 
        /* height: 25px; */
    }
</style>