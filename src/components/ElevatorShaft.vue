<template>
    <div class="elevator-shaft">
        <div v-for="(floor, i) in floors" :key="i" class="floor">
            {{ floor }}
            <div v-if="floor === 1" class="elevator"></div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        floors: {
            type: Number,
            required: true,
        },
    },
};
</script>

<style scoped>
.elevator-shaft {
    box-sizing: inherit;
    display: flex;
    flex-direction: column-reverse;
    border: 1px solid black;
    width: 100%;
    height: 100%;
}
.floor {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
}
.floor:not(:last-child):before {
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
    position: absolute;

    z-index: -3;
}
</style>
