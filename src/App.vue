<template>
  <div class="all"> 
    <!-- Меню с кнопками -->
    <WidjetsMenu @addText="addText"></WidjetsMenu>
    <!-- Общее поле, где можно перемещать элементы -->
    <div
      class="container"
      @drop="onDrop($event)" 
      @dragover.prevent
      @dragenter.prevent
    >
    <!-- Блок с текстом -->
    <!-- id, xCoord, yCoord это пропсы из TextBlock.vue -->
      <div v-if="items.length > 0">
        <TextBlock
          v-for="item in items"
          :key="item.id"
          :xCoord="item.xCoord"
          :yCoord="item.yCoord"
          :id="item.id"
          :fontSize="item.fontSize"
          :fontColor="item.color"
          @objectSelection="objectSelection"
        ></TextBlock>
      </div>
    </div>
    <div class="objectMenu">
      <MenuText v-if="selectedObject == 'TextBlock'"
                :id="selectedObjectId"
                :fontSizeElement="this.items.find((item) => item.id == selectedObjectId).fontSize"
                @changeFontSize="changeFontSize"
                @deleteElement="deleteElement"
                @changeColorText="changeColorText"
                ></MenuText>
                
    </div>
  </div>
</template>

<script>

import TextBlock from '@/components/TextBlock.vue'
import WidjetsMenu from '@/components/WidjetsMenu.vue'
import MenuText from '@/components/MenuText.vue'


export default {
  name: 'App',
  components: {
   TextBlock,
   WidjetsMenu,
   MenuText,
  },
  data (){
    return{
      // Массив объектов, где хранятся данные об элементе
      items: [
        // {
          // id: 0, 
          // xCoord: 5, // коорды по x и y в %, нужны для атрибутов left и top
          // yCoord: 20,
          // fontSize: 10,
        // }
      ],
      selectedObject: "none",
      selectedObjectId : 0,
    }
  },
  methods: {
    // При "броске элемента"
    onDrop(event){
      const itemId = event.dataTransfer.getData('itemId') // Достаём id перетаскиваемого элемента, который записали в drug
      const item = this.items.find((item) => item.id == itemId) // Ищем из массива объектов 
      // Изменяем координаты элемента на разницу в координатах мыши
      // все данные вычисляются в % от всего окна
      item.xCoord += (event.clientX / document.documentElement.clientWidth * 100) - event.dataTransfer.getData('xCurrent')
      item.yCoord += (event.clientY / document.documentElement.clientHeight * 100) - event.dataTransfer.getData('yCurrent') 

      if(item.yCoord < 14.5){ // Ограничение элемента при перемещении вверх, нужно регулировать в зависимости от размера меню
        item.yCoord = 14.5
      }
      if(item.xCoord < 0){ // Ограничение элемента при перемещении влево
        item.xCoord = 0
      }
      // Ограничение элемента при перемещении вправо
      // Здесь достаётся ширина объекта, записанного в drug
      // 99.3 было подобрано в ручную, ибо при 100 элемент вылезает немного вправо
      if(item.xCoord + (parseInt(event.dataTransfer.getData('width')) / document.documentElement.clientWidth * 100) > 100){
        item.xCoord = 99.3 - parseInt(event.dataTransfer.getData('width')) / document.documentElement.clientWidth * 100
      }

      console.log('Упал')
      
    },
    addText(){ // Добавление текстового блока
      this.items.push({ // Запись нового элемента в массив объектов
        id: this.items.length, 
        xCoord: 5,
        yCoord: 20,
        fontSize: 10,
        color: '#000000',
      })
    },
    objectSelection(typeObject, id){
      this.selectedObject = typeObject
      this.selectedObjectId = id
    },
    changeFontSize(fontSizeValue, idElement){
      const item = this.items.find((item) => item.id == idElement)
      item.fontSize = fontSizeValue
    },
    changeColorText(idElement, colorElement){
      const item = this.items.find((item) => item.id == idElement)
      item.color = colorElement
    },
    deleteElement(idElement){
      this.selectedObject = 'none'
      this.items.splice(idElement, 1)
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin: 0;
}

.container{
  background-color:aqua;
  height:800px;
  width: 70%;
}

.objectMenu{
  background-color: blueviolet;
  width: 20%;
  border: 2px solid black;
}

.all{
  display: flex;
}

body{
  margin: 0;
  padding: 0;
}


</style>
