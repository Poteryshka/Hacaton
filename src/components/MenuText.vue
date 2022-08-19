<template>
    <div class="widjetsMenu">
        <div>
            <p>Размер шрифта: {{fontSizeElement}}px</p>
        </div>
        <input class="range" type="range" min="1" max="100" :value="fontSizeElement"
                @change="changeFontSize($event)"/>

        <div class="palitra">
            <div 
                v-for="color in colorBase"
                @click="changeColorText(color)" 
                :key="color"
                class="palitraElement"
                :id="color"
                :style="{'background-color': color}">
            </div>
        </div>

        <div>
            <p>Высота: </p> 
            <input value="50" class="enter">
        </div>
        <div>
            <p>Ширина: </p>
            <input class="enter">
        </div>
        <div>
            <p>X: </p>
            <input class="enter"
                   :value="xElement"
                   @change="changeXElement($event)">
        </div>
        <div>
            <p>Y: </p>
            <input class="enter"
                   :value="yElement"
                   @change="changeYElement($event)">
        </div>
        
        <button @click="deleteElement">Удалить</button>

        
    </div>
</template>

<script>
    export default{
        props: {
            id: Number,
            fontSizeElement: Number,
            widthElement: Number,
            heigthElement: Number,
            xElement: Number,
            yElement: Number,
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
            },
            changeXElement(event){
                // console.log("MENU x: " + event)
                this.$emit('changeXElement', this.id, Number(event.target.value))
            },
            changeYElement(event){
                this.$emit('changeYElement', this.id, Number(event.target.value))
            },
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
        width:95%;
        padding-left: 5%;
    }

    p{
        font-size: 0.9em;
        font-family:Verdana, Geneva, Tahoma, sans-serif;
        color: #D4E6ED;
        margin-top: 10%;
        display: inline-block;
    }

    .palitra{
        margin-top: 10%;
        width: 90%;
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
        /* display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr; */
    }

    .palitraElement{
        /* border: 1px solid black; */
        padding: 6%;
        margin: 3%;
        border-radius: 50%;
        /* height: 25px; */
    }

    input.enter{
        width: 20%;
        border: 1px solid #2A4164; 
        border-radius: 5%;
        display: inline-block;
    }

    button{
        margin-top: 10%;
        background-color: #177ab3;
        border: 2px solid #FFFFFF ;
        color: #FFFFFF;
        border-radius: 4%;
        padding: 3%;
        font-family:Verdana, Geneva, Tahoma, sans-serif;
    }

    button:hover{
        background-color: #2e80b0;
        color: #185578;
    }

    input[type=range] {
    -webkit-appearance: none; /* Скрывает слайдер, чтобы можно было создать свой */
    width: 50%; /* Указание параметра ширины требуется для Firefox. */
    }
    
    input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none;
    }
    
    input[type=range]:focus {
    outline: none; /* Убирает голубую границу у элемента. Хотя, возможно, и стоит создавать некоторое оформления для состояния фокуса в целях обеспечения доступности. */
    }
    
    input[type=range]::-ms-track {
    width: 100%;
    cursor: pointer;
    background: transparent; /* Скрывает слайдер, чтобы можно было добавить собственные стили. */
    border-color: transparent;
    color: transparent;
    }

input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
  border: 1px solid #134969;
  height: 15px;
  width: 16px;
  border-radius: 50%;
  background: #2796d6;
  cursor: pointer;
  margin-top: -5px; 
  margin-bottom: -5px;

}
</style>