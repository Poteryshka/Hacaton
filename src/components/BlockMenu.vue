<template>
    <div class="widjetsMenu">
        <p>Размер шрифта: {{fontSize}}px</p>

        <input type="range" min="1" max="100"
                v-model="fontSize"/>

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

        <div>
            <div>
                <p>Ширина</p>
                <input type="input" v-model="width"/>
                <p>Высота</p>
                <input type="input" v-model="height"/>

                <p>X</p>
                <input type="input" v-model="x"/>
                <p>Y</p>
                <input type="input" v-model="y"/>
            </div>
        </div>
        
        <button @click="deleteElement">Удалить</button>

        
    </div>
</template>

<script>
    export default {
        data: () => ({
            width: 0,
            height: 0,
            fontSize: 0,
            x: 0,
            y: 0,
            colorBase: ['#FFFFFF', '#000000', '#808080', '#C0C0C0', '#FF00FF', 
                            '#800080', '#000080', '#FF0000', '#FFFF00', '#808000',
                            '#00FF00', '#008000', '#0000FF', '#FF4500', '#8B0000'],
        }),
        methods: {
            changeSize (data) {
                this.$emit('changeSize', data)
            },
            deleteElement(){
                this.$emit('deleteElement', this.id)
            },
            changeStyle (data){
                this.$emit('changeStyle', data)
            },
            changeColorText (newValue) {
                this.changeStyle ({
                    color: newValue
                })
            }
        },
        watch: {
            width (newValue) {
                this.changeSize({
                    width: newValue
                })
            },
            height (newValue) {
                this.changeSize({
                    height: newValue
                })
            },
            fontSize (newValue) {
                this.changeStyle({
                    fontSize: newValue
                })
            },
        },
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