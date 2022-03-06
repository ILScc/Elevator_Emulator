<template>
    <div class="container">
        <ElevatorShaft
            v-for="{ shaftId, currentFloor, isReady } in shaftsToRender"
            :key="shaftId"
            :floors="floors"
            :destinationFloor="isReady ? earliestCall : null"
            :isReady="isReady"
            :currentFloor="currentFloor"
            :shaftId="shaftId"
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
            shaftsToRender: [],
            totalShafts: 4,
            floors: 5,
            callsQueue: [],
            callStatus: { calledFloor: null, accepted: true, reason: "" },
        };
    },
    mounted() {
        this.shaftsToRender = this.createShafts();
        const callsState = localStorage.getItem("callsQueue");

        for (let i = 1; i <= this.totalShafts; i++) {
            const savedCurrentFloor = localStorage.getItem(`shaft${i}`);
            if (savedCurrentFloor) {
                const currentFloor = JSON.parse(savedCurrentFloor);
                const shaft = this.findShaft(i);
                shaft.currentFloor = currentFloor;
            }
        }
        if (callsState) {
            const queue = JSON.parse(callsState);
            this.callsQueue = queue;
        }
    },
    methods: {
        findShaft(id) {
            return this.shaftsToRender.find((shaft) => shaft.shaftId === id);
        },
        createShafts() {
            const shaftsToRender = [];
            for (let i = 1; i <= this.totalShafts; i++) {
                const savedCurrentFloor = JSON.parse(
                    localStorage.getItem(`${i}currentFloor`)
                );
                const currentFloor = savedCurrentFloor ? savedCurrentFloor : 1;
                shaftsToRender.push({
                    shaftId: i,
                    currentFloor,
                    isReady: true,
                });
            }
            return shaftsToRender;
        },

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
            const alreadyOnFloor = this.shaftsToRender.find(
                (shaft) => shaft.currentFloor === floor
            );
            if (alreadyOnFloor) {
                this.validateCall(floor, false, "ALREADY ON FLOOR");
                return;
            }
            this.validateCall(floor, true);
            this.callsQueue.push(floor);
        },
        onElevatorReady([shaftId]) {
            const shaft = this.findShaft(shaftId);
            shaft.isReady = true;
            this.callsQueue.shift();
        },
        onElevatorLanded([shaftId, currentFloor]) {
            const shaft = this.findShaft(shaftId);
            shaft.isReady = false;
            shaft.currentFloor = currentFloor;
            localStorage.setItem(
                `shaft${shaftId}`,
                JSON.stringify(currentFloor)
            );
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
