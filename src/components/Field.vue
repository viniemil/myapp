<template>
    <div class="square" @click="onSquareClick">{{ squareValue }}</div>
</template>
 
<script>
export default {
    name: "Square-Comp",
    props: {
        squareValue: String,
        actionFill: Function,
    },
    methods: {
        onSquareClick() {
            this.$emit('actionFill');
        }
    },

}
</script>

<style scoped>
.square {
    background-color: #fff;
    width: 40px;
    height: 40px;
    border: 1px solid #61dafb;
    text-align: center;
    font-weight: bold;
    line-height: 40px;
}
</style>
