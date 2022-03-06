<template>
    <div class="elevator">
        <div class="elevator__cabin">
            <div v-if="!isReady" class="elevator__arrow">
                <img
                    v-if="elevatorUp"
                    src="../assets/ArrowUp.svg"
                    alt="arrow up"
                />
                <img
                    v-else-if="elevatorDown"
                    src="../assets/ArrowDown.svg"
                    alt="arrow down"
                />
            </div>
            {{ currentFloor }}
        </div>
    </div>
</template>

<script>
export default {
    props: {
        currentFloor: {
            type: Number,
            required: true,
        },
        prevFloor: {
            type: Number,
            required: true,
        },
        isReady: {
            type: Boolean,
            requred: true,
        },
    },
    computed: {
        elevatorUp() {
            return this.currentFloor > this.prevFloor;
        },
        elevatorDown() {
            return this.currentFloor < this.prevFloor;
        },
    },
};
</script>

<style scoped>
.elevator {
    background-color: aqua;
    width: 100%;
    height: 100%;
    position: absolute;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: -3;
}
.elevator__cabin {
    margin-top: 5px;
    width: 50px;
    height: 50px;
    border: 1px solid blue;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
</style>
