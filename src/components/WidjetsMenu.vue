<template>
    <div class="menu">
        <div class="buttonContainer" @click="addText">
            <img src="@/assets/image/iconBackgroud.png">
            <p>Задний фон</p>
        </div>
        <div class="buttonContainer" @click="addText">
            <img src="@/assets/image/iconText.png">
            <p>Текст</p>
        </div>
        <div class="buttonContainer">
            <img src="@/assets/image/iconImage.png">
            <p>Картинка</p>
        </div>
        <div class="buttonContainer">
            <img src="@/assets/image/iconFigure.png">
            <p>Фигура</p>
        </div>
        <div class="buttonContainer">
            <img src="@/assets/image/iconSlider.png">
            <p>Слайдер</p>
        </div>
        <div class="buttonContainer">
            <img src="@/assets/image/iconForm.png">
            <p>Форма</p>
        </div>
        <div class="buttonContainer">
            <img src="@/assets/image/iconButton.png">
            <p>Кнопка</p>
        </div>
        <div class="buttonContainer">
            <img src="@/assets/image/iconInput.png">
            <p>Поле ввода</p>
        </div>
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
        /* padding: 15px; */
        background-color: #1C2A33;
        width: 10%;
    }

    .buttonContainer{
        margin: 7% 20% 0% 20%; /* top | right | bottom | left */
        width: 60%;
        background-color: #263845;
        text-align: center;
        padding-bottom: 5%;
    }

    .buttonContainer:hover{
        background-color: #49677C;
    } 

    p{
        color: #D4E6ED;
        font-family: Arial, Helvetica, sans-serif;
        src: local('Extra light italic'),
		url(@/assets/Exo_2_Extra_Light.ttf); 
        font-size: 0.8em;
        margin-top: 10%;
        margin-bottom: 0; 
    }

    img{
        margin-top: 10%;
    }

    button{
        height: 50px;
        margin-right: 10px;
    }
</style>