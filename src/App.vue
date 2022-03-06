<template>
    <div class="container">
        <ElevatorShaft
            v-for="shaft in elevatorShafts"
            :key="shaft"
            :floors="floors"
            :destinationFloor="earliestCall"
            @elevator-ready="onElevatorReady"
            @elevator-landed="onElevatorLanded"
        />
        <ElevatorControls
            :callStatus="callStatus"
            @elevator-called="handleCall"
            :floors="floors"
        />
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
            occupiedFloor: 1,
            callStatus: { calledFloor: null, accepted: true, reason: "" },
        };
    },
    methods: {
        handleCall(floor) {
            if (this.callsQueue.includes(floor)) {
                this.callStatus.accepted = false;
                this.callStatus.calledFloor = floor;
                this.callStatus.reason = "ALREADY CALLED";
                return;
            }
            if (this.occupiedFloor === floor) {
                this.callStatus.accepted = false;
                this.callStatus.calledFloor = floor;
                this.callStatus.reason = "ALREADY ON FLOOR";
                return;
            }
            this.callStatus.accepted = true;

            this.callsQueue.push(floor);
        },
        onElevatorReady() {
            this.callsQueue.shift();
        },
        onElevatorLanded(currentFloor) {
            this.occupiedFloor = currentFloor;
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
