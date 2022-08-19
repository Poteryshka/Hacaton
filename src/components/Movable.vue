<template>
        <div
            :class="{
                'block': true,
                'block-selected': isElSelected || isDrag,
            }" 
            :style="[movableStyles, transform]"
            tabindex="0"
            @mousedown="dragMouseDown"
            @mousemove="dragMouseMove"
            @mouseup="dragMouseUp"
            @focus="objectSelection"
            @blur="objectUnselect"
        >
            <div v-if="(isElSelected && !isDrag) || isSizeChange">
                <div
                    v-for="pointer in sizeChangePointers"
                    :key="pointer.direction"
                    :class="['pointer', pointer.direction]"
                    :style="pointer.style"
                    @mousedown="sizeChangeMouseDown($event, pointer.direction)"
                    @mouseup="sizeChangeMouseUp"
                    @mousemove="sizeChangeMouseMove"
                >
                </div>

                <div
                    class="pointer rotate-pointer"
                    :style="rotatePointerStyle"
                    @mousedown="rotateMouseDown"
                    @mouseup="rotateMouseUp"
                    @mousemove="rotateMouseMove"
                >
                </div>
            </div>
        </div>
</template>

<script>

export default{
    name: 'MovableBlock',
    props: {
        id: {
            type: Number,
        },
        pos: {
            default: () => ({
                x: 250,
                y: 250,
                rotation: 0,
            })
        },
        styles: {
            default: () => ({
                fontSize: '24px',
                color: 'black',
            })
        },
        size: {
            default: () => ({
                width: 200,
                height: 100,
            })
        },
        gridSize: {
            default: 20
        }
    },
    data: () => ({
        sizeChangePointers: [],
        rotatePointerStyle: {},
        pointerSize: 12,
        directionMap: [
            'nw',
            'n',
            'ne',
            'e',
            'se',
            's',
            'sw',
            'w'
        ],
        isElSelected: false,
        innerPos: {},
        innerSize: {
            width: 0,
            height: 0
        },
        mouseStartPos: {
            x: 0,
            y: 0
        },
        startSize: {},
        isDrag: false,
        isSizeChange: false,
        isRotate: false,
        prevValue: {
            x: 0,
            y: 0
        },
        curDir: '',
    }),
    mounted () {
        document.documentElement.addEventListener('mouseup', this.dragMouseUp, true)
        document.documentElement.addEventListener('mouseup', this.sizeChangeMouseUp, true)
        document.documentElement.addEventListener('mouseup', this.rotateMouseUp, true)
        document.documentElement.addEventListener('mousemove', this.dragMouseMove, true)
        document.documentElement.addEventListener('mousemove', this.sizeChangeMouseMove, true)
        document.documentElement.addEventListener('mousemove', this.rotateMouseMove, true)
        this.innerSize = this.size
        this.innerPos = this.pos
        for (let direction of this.directionMap) {
            let pointer = {}
            pointer.style = {
                top: 'calc(' + (direction.includes('n') ? '0%' : (direction.includes('s') ? '100%' : '50%')) + ' - ' + Math.floor(this.pointerSize / 2 + 1) + 'px)',
                left: 'calc(' + (direction.includes('w') ? '0%' : (direction.includes('e') ? '100%' : '50%')) + ' - ' + Math.floor(this.pointerSize / 2 + 1) + 'px)',
                cursor: direction + '-resize',
                width: this.pointerSize + 'px',
                height: this.pointerSize + 'px',
            }
            pointer.direction = direction
            this.sizeChangePointers.push(pointer)
        }
        this.rotatePointerStyle = {
            width: this.pointerSize + 'px',
            height: this.pointerSize + 'px',
            top: '-2rem',
            left: 'calc(50% - ' + Math.round(this.pointerSize / 2 + 1) + 'px)',
        }
    },
    computed: {
        movableStyles () {
            return Object.assign(this.styles, this.styleSize, {position: 'absolute'})
        },
        styleSize () {
            return {
                width: this.innerSize.width + 'px',
                height: this.innerSize.height + 'px',
            }
        },
        transform () {
            return {
                top: (this.innerPos.y + 'px'),
                left: (this.innerPos.x + 'px'),
                transform: 'rotate(' + this.innerPos.rotation + 'deg)',
            }
        },
    },
    methods: {
        dragMouseDown (event) {
            this.mouseStartPos.x = this.innerPos.x - event.clientX
            this.mouseStartPos.y = this.innerPos.y - event.clientY
            this.$emit('stateChange');
            this.isDrag = true
        },
        dragMouseMove (event) {
            if (this.isDrag) {
                this.innerPos.x = (event.clientX + this.mouseStartPos.x) - (event.clientX + this.mouseStartPos.x) % this.gridSize
                this.innerPos.y = (event.clientY + this.mouseStartPos.y) - (event.clientY + this.mouseStartPos.y) % this.gridSize
            }
        },
        dragMouseUp () {
            this.$emit('stateChange')
            this.isDrag = false
        },
        objectSelection () {
            this.isElSelected = true
            this.$emit('objectSelected')
        },
        objectUnselect () {
            this.isElSelected = false
            this.$emit('objectUnselected')
        },
        sizeChangeMouseDown (event, direction) {
            this.isSizeChange = true
            this.curDir = direction
            setTimeout(() => {
                this.isDrag = false
                this.mouseStartPos.x = event.clientX
                this.mouseStartPos.y = event.clientY
            }, 20)
            this.startSize.width = this.prevValue.x = this.innerSize.width
            this.startSize.height = this.prevValue.y = this.innerSize.height
        },
        sizeChangeMouseMove (event) {
            if (this.isSizeChange) {
                if (this.curDir.includes('w') || this.curDir.includes('e')) {
                    let diffX = 0
                    if (this.curDir.includes('w')) {
                        diffX = (this.startSize.width + (event.clientX - this.mouseStartPos.x) * -1) - (this.startSize.width + (event.clientX - this.mouseStartPos.x) * -1) % this.gridSize
                        this.innerPos.x += this.prevValue.x - diffX
                    } else {
                        diffX = (this.startSize.width + (event.clientX - this.mouseStartPos.x)) - (this.startSize.width + (event.clientX - this.mouseStartPos.x)) % this.gridSize
                    }
                    if (diffX >= this.gridSize) {
                        this.innerSize.width = diffX
                    }
                    this.prevValue.x = diffX
                }
                if (this.curDir.includes('n') || this.curDir.includes('s')) {
                    let diffY = 0
                    if (this.curDir.includes('n')) {
                        diffY = (this.startSize.height + (event.clientY - this.mouseStartPos.y) * -1) - (this.startSize.height + (event.clientY - this.mouseStartPos.y) * -1) % this.gridSize
                        this.innerPos.y += this.prevValue.y - diffY
                    } else {
                        diffY = (this.startSize.height + (event.clientY - this.mouseStartPos.y)) - (this.startSize.height + (event.clientY - this.mouseStartPos.y)) % this.gridSize
                    }
                    if (diffY >= this.gridSize) {
                        this.innerSize.height = diffY
                    }
                    this.prevValue.y = diffY
                }
            }
        },
        sizeChangeMouseUp () {
            this.isSizeChange = false
        },
        rotateMouseDown (event) {
            setTimeout(() => {
                this.isDrag = false
                this.isRotate = true
                this.isElSelected = true
                this.mouseStartPos.x = event.clientX
                this.mouseStartPos.y = event.clientY
            }, 40)
            this.$emit('stateChange');
        },
        rotateMouseMove (event) {
            if (this.isRotate) {
                this.innerPos.rotation = Math.atan((event.clientX - this.mouseStartPos.x) / (event.clientY - this.mouseStartPos.y)) * 180 / Math.PI
                console.log(this.innerPos.rotation)
            }
        },
        rotateMouseUp () {
            this.$emit('stateChange')
            this.isRotate = false
        },
    },

    watch: {
        pos: {
            handle (newValue) {
                this.innerPos = newValue
            },
            deep: true
        },
        size: {
            handle (newValue) {
                this.innerSize = newValue
            },
            deep: true
        }
    },
}

</script>

<style scoped>
.block{
    text-align: left;
    display: inline-block;
    background: none;
    width: 200px;
    height: 100px;
    font-size: 10px;
    z-index: 1;
    margin: 0;
    padding: 0;
    border: 1px solid #333;
    cursor: move;
}

.block-selected {
    border: 2px dotted #333;
}

.pointer {
    background: #CCC;
    position: absolute;
    border-radius: 50%;
    border: 1px solid #444;
    z-index: 2;
    margin: 0;
    padding: 0;
}

.rotate-pointer {
    cursor: crosshair;
}
</style>
