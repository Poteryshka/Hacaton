<template>
        <textarea 
            class="text" 
            :style="{ top: yCoord + '%', left: xCoord + '%', 'font-size': fontSize + 'px', color: fontColor}"
            draggable="true" 
            @dragstart="startDrag($event, id)"
            @click="objectSelection"
            >
        </textarea>
</template>

<script>

export default{
    name: 'TextBlock',
    props: {
        // id и коорды каждого элемента
        id: Number,
        xCoord: Number,
        yCoord: Number,
        fontSize: Number,
        fontColor: String
    },
    methods: {
        startDrag(event, id){
            // Изменение визуальной части перетаскивания
            event.dataTransfer.dropEffect = 'move'
            event.dataTransfer.effectAllowed = 'move'
            // Запись в dataTransfer id и коорды начального положения мышки
            event.dataTransfer.setData('itemId', id)
            event.dataTransfer.setData('xCurrent', event.clientX / document.documentElement.clientWidth * 100)
            event.dataTransfer.setData('yCurrent', event.clientY / document.documentElement.clientHeight * 100)
            // При создании элемента, его высота и ширина не записывается, в таком случае записываем это вручную    
            if(!event.target.style.width && !event.target.style.height){
                event.target.style.width = '200px'
                event.target.style.height = '100px'
            }
            // Передаём в dataTransfer размеры элемента
            event.dataTransfer.setData('width', event.target.style.width)
            event.dataTransfer.setData('height', event.target.style.height)

        },
        objectSelection(){
            this.$emit('objectSelection', "TextBlock", this.id)
        },
    }
}

</script>

<style scoped>
    .text{
        text-align: left;
        position: absolute;
        display: inline-block;
        background: none;
        border: 2px dotted black;
        overflow: hidden;
        width: 200px;
        height: 100px;
        font-size: 10px;
    }

</style>