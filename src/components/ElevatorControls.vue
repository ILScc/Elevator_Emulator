<template>
    <div class="elevator-controls">
        <div
            v-for="(floor, i) in floors"
            :key="i"
            class="elevator-controls__container"
        >
            <button class="elevator-controls__btn">
                Call elevator on floor {{ floor }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props:{
        floors: {
            type: Number,
            required: true
        }
    }
};
</script>

<style>
.elevator-controls {
    display: flex;
    flex-direction: column-reverse;
    border: 1px solid tomato;
}
.elevator-controls__btn {
    width: 30%;
    height: 30%;
}
.elevator-controls__container {
    margin-left: 10px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 100%;
    height: 100%;
}
</style>
