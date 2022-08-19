<template>
    <div>
        <Movable 
            :id="id"
            :size="size"
            :styles="styles"
            :pos="pos"
            @object-selected="objectSelection"
        >
            <slot />
        </Movable>
        <div v-if="id == selectedObjectId">
            <Teleport to="#optionsMenu">
                <BlockMenu
                    @deleteElement="deleteElement"
                    @changeSize="changeSize"
                    @changeStyle="changeStyle"
                    @changePos="changePos"
                ></BlockMenu>
            </Teleport>
        </div>
    </div>
</template>

<script>
import Movable from '@/components/Movable.vue'
import BlockMenu from '@/components/BlockMenu.vue'

export default {
    components: {
        Movable,
        BlockMenu,
    },
    props: {
        id: {
            type: Number
        },
        selectedObjectId: {
            type: Number
        }
    },

    data: () => ({
        size: {
            width: 100,
            height: 100,
        },
        styles: {
            fontSize: '12px',
            color: 'black',
        },
        pos: {
            x: 200,
            y: 200,
            rotation: 0
        },
        isSelected: false,
    }),

    methods: {
        objectSelection () {
            this.$emit('objectSelection', this.id);
        },
        changeStyle (data) {
            this.styles[Object.keys(data)[0]] = data[Object.keys(data)[0]]
        },
        changeSize (data) {
            this.size[Object.keys(data)[0]] = data[Object.keys(data)[0]]
        },
        changePos (data) {
            this.pos[Object.keys(data)[0]] = data[Object.keys(data)[0]]
        },
        deleteElement () {
            this.$emit('deleteBlock', this.id)
        },
    }
}
</script>

<style>
</style>