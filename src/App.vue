<template>
    <div class="container">
        <ElevatorShaft
            v-for="shaft in elevatorShafts"
            :key="shaft"
            :floors="floors"
            :destinationFloor="earliestCall"
            :currentFloor="currentFloor"
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
            currentFloor: 1,
            callStatus: { calledFloor: null, accepted: true, reason: "" },
        };
    },
    mounted() {
        const callsState = localStorage.getItem("callsQueue");
        const savedCurrentFloor = localStorage.getItem("currentFloor");

        if (callsState && savedCurrentFloor) {
            const queue = JSON.parse(callsState);
            const currentFloor = JSON.parse(savedCurrentFloor);
            this.currentFloor = currentFloor;
            this.callsQueue = queue;
        }
    },
    methods: {
        validateCall(calledFloor, isAccepted, rejectReason) {
            this.callStatus = {
                calledFloor,
                accepted: isAccepted,
                reason: rejectReason,
            };
        },
        handleCall(floor) {
            if (this.callsQueue.includes(floor)) {
                this.validateCall(floor, false, "ALREADY CALLED");
                return;
            }
            if (this.currentFloor === floor) {
                this.validateCall(floor, false, "ALREADY ON FLOOR");
                return;
            }
            this.validateCall(floor, true);
            this.callsQueue.push(floor);
        },
        onElevatorReady() {
            this.callsQueue.shift();
        },
        onElevatorLanded(currentFloor) {
            this.currentFloor = currentFloor;
        },
    },
    computed: {
        earliestCall() {
            return this.callsQueue[0];
        },
    },
    watch: {
        callsQueue: {
            handler() {
                localStorage.setItem(
                    "callsQueue",
                    JSON.stringify(this.callsQueue)
                );
                localStorage.setItem("currentFloor", this.currentFloor);
            },
            deep: true,
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
