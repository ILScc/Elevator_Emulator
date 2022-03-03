<template>
    <div class="container">
        <div class="elevator-shaft">
            <div class="floor"></div>
            <div class="floor"></div>
            <div class="floor"></div>
            <div class="floor"></div>
            <div class="floor"><div class="elevator"></div></div>
        </div>
        <div class="elevator-controls"></div>
    </div>
</template>

<script>
export default {};
</script>

<style scoped>
.container {
    display: grid;
    width: 1200px;
    height: 600px;
    grid-template-columns: 1fr 2fr;
    grid-template-rows: 1fr;
    box-sizing: border-box;
}
.elevator-shaft {
    box-sizing: inherit;
    display: grid;
    border: 1px solid black;
    width: 100%;
    height: 100%;
}
.floor {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
}
.floor:not(:first-child):before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    width: 100%;
    border: 1px dashed blue;
}
.elevator {
    background-color: aqua;
    width: 100%;
    height: 100%;
}
.elevator-controls {
    box-sizing: inherit;
    border: 1px solid tomato;
}
</style>
