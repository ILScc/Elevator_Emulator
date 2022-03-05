<template>
    <div class="container">
        <ElevatorShaft
            v-for="shaft in elevatorShafts"
            :key="shaft"
            :floors="floors"
            :destinationFloor="earliestCall"
            @floor-set="handleFloorSet"
        />
        <ElevatorControls @elevator-called="handleCall" :floors="floors" />
    </div>
</template>

<script>
import ElevatorShaft from "./components/ElevatorShaft.vue";
import ElevatorControls from "./components/ElevatorControls.vue";
export default {
    components: { ElevatorShaft, ElevatorControls },
    data() {
        return {
            elevatorShafts: 1,
            floors: 5,
            callsQueue: [],
        };
    },
    methods: {
        handleCall(floor) {
            this.callsQueue.push(floor);
        },
        handleFloorSet() {
            this.callsQueue.shift();
        },
    },
    computed: {
        earliestCall() {
            return this.callsQueue[0];
        },
    },
};
</script>

<style scoped>
.container {
    margin: 50px;
    display: grid;
    width: 1200px;
    height: 750px;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    grid-template-rows: 1fr;
    box-sizing: border-box;
}
</style>
