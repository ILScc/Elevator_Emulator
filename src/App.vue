<template>
    <div class="container">
        <div class="elevator-shaft">
            <div v-for="(floor, i) in floors" :key="i" class="floor">
                {{ floor }}
                <div v-if="floor === 1" class="elevator"></div>
            </div>
        </div>
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
    </div>
</template>

<script>
export default {
    data() {
        return {
            elevatorShafts: 1,
            floors: 5,
        };
    },
};
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
