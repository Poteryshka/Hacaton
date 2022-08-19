<template>
  <!-- Меню с виджетами -->
  <WidjetsMenu @click="addText"></WidjetsMenu>  
 
  <!-- Поле с текстовыми элементами -->
  <div class="droppable pole1" 
       @drop="onDrop($event, 1)" 
       @dragover.prevent
       @dragenter.prevent>
    <TextBlock v-for="item in listOne" :key="item.id"  
               class="draggable" 
               draggable="true" 
               @change="changeText($event, item)"
               @dragstart="startDrag($event, item)"
               :text="item."></TextBlock>  
  </div>

  <div class="droppable pole2" 
       @drop="onDrop($event, 2)"
       @dragover.prevent
       @dragenter.prevent>
    <TextBlock v-for="item in listTwo" :key="item.id"  
               class="draggable" 
               draggable="true" 
               @change="changeText($event, item)"
               @dragstart="startDrag($event, item)"
               ></TextBlock>
  </div>

  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TextBlock from '@/components/Movable.vue'
import WidjetsMenu from '@/components/WidjetsMenu.vue'

export default {
  name: 'App',
  components: {
    TextBlock,
    WidjetsMenu,
  },
    
  data() {
    return {
      items: [
        {
          id: 0,
          list: 1,
        },
        {
          id: 1,
          list: 1,
        },
        {
          id: 2,
          list: 2,
        },
      ],
    }
  },

  computed: {
    listOne() {
      return this.items.filter((item) => item.list ===1)
    },
    listTwo() {
      return this.items.filter((item) => item.list === 2)
    },
  },

  methods: {
    startDrag(event, item){
      event.dataTransfer.dropEffect = 'move'
      event.dataTransfer.effectAllowed = 'move'
      event.dataTransfer.setData('itemID', item.id)
    },
    onDrop(event, list) {
      const itemID = event.dataTransfer.getData('itemID')
      const item = this.items.find((item) => item.id == itemID)
      item.list = list
    },
    changeText(event, item){
      item.title = event.target.value
      this.items[item.id] = item

      // console.log(event.target.cols)
      // console.log(event.target.rows)
      // console.log(event.target.value)

    },
    addText(){
      this.items.push({
        id: this.items.length,
        title: 'text',
        list: 1,
      })
      // console.log(this.listOne)
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.pole1{
  /* height: 1000px; */
  background-color: orange;
  min-height: 50px;
  border: black 4px solid;
  border-radius: 10px;
  margin-bottom: 3%;
  margin-top: 3%;
}

.pole2{
  min-height: 50px;
  background-color: blueviolet;
  border: black 4px solid;
  border-radius: 10px;
  padding: 5px;
}
</style>

<!-- ТЕКСТ БЛОК -->

<template>
    <div>
             <textarea :rows="rows" :cols="cols" v-model="text"></textarea>
        <!-- <input :value="text"> -->
        <!-- <p>{{text}}</p> -->
    </div>
</template>

<script>

export default{
    name: 'TextBlock',
    data() {
        return{
            text: '',
            cols: 10,
            rows: 1,
        }
    },
    methods: {
        changeText(event, item){
            console.log(this)
            this.$emit('changeText',event, item)
        }
    }
}

</script>

<style scoped>
    div{
        border: 2px solid black;
        margin: 5px;
        text-align: left;
        display: inline-block;
        padding: 5px;
    }

    textarea{
        background: none;
        border: none;
        height: auto;
    }
</style>
