<template>
    <div class="elevator">
        <div class="elevator__cabin">
            <div class="elevator__arrow">
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
            {{ destination }}
        </div>
    </div>
</template>

<script>
export default {
    props: {
        destination: {
            type: Number,
            required: false,
        },
        currentFloor: {
            type: Number,
            required: true,
        },
    },
    computed: {
        elevatorUp() {
            return this.currentFloor < this.destination;
        },
        elevatorDown() {
            return this.currentFloor > this.destination;
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
