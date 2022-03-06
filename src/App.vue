<template>
    <div class="container">
        <ElevatorShaft
            v-for="{ shaftId, currentFloor, isReady } in shaftsToRender"
            :key="shaftId"
            :floors="floors"
            :destinationFloor="
                closestToDestination === shaftId ? earliestCall : null
            "
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
            totalShafts: 3,
            floors: 8,
            callsQueue: [],
            callStatus: { calledFloor: null, accepted: true, reason: "" },
        };
    },
    mounted() {
        this.shaftsToRender = this.createShafts();
        const callsState = localStorage.getItem("callsQueue");
        if (callsState) {
            const queue = JSON.parse(callsState);
            this.callsQueue = queue;
        }
        this.callsQueue.push("");
        this.callsQueue.pop();
    },
    methods: {
        findShaft(id) {
            return this.shaftsToRender.find((shaft) => shaft.shaftId === id);
        },
        createShafts() {
            const shaftsToRender = [];
            for (let i = 1; i <= this.totalShafts; i++) {
                const savedCurrentFloor = JSON.parse(
                    localStorage.getItem(`shaft${i}`)
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
        },
        onElevatorLanded([shaftId, currentFloor]) {
            const targetShaft = this.findShaft(shaftId);
            targetShaft.isReady = false;
            targetShaft.currentFloor = currentFloor;
            localStorage.setItem(
                `shaft${shaftId}`,
                JSON.stringify(currentFloor)
            );
            this.callsQueue.shift();
        },
    },
    computed: {
        earliestCall() {
            return this.callsQueue[0];
        },
        closestToDestination() {
            const readyShafts = this.shaftsToRender.filter(
                ({ isReady }) => isReady
            );
            const minDiff = Math.min(
                ...Object.values(
                    readyShafts.map((shaft) =>
                        Math.abs(this.earliestCall - shaft.currentFloor)
                    )
                )
            );
            const minDiffShafts = [];
            readyShafts.forEach(({ shaftId, currentFloor, isReady }) => {
                const diff = Math.abs(this.earliestCall - currentFloor);
                if (diff === minDiff && isReady) {
                    minDiffShafts.push(shaftId);
                }
            });
            console.log("ALl", minDiffShafts);

            return Math.min(...minDiffShafts);
        },
    },
    watch: {
        callsQueue: {
            handler(newVal, prevVal) {
                localStorage.setItem(
                    "callsQueue",
                    JSON.stringify(this.callsQueue)
                );
                console.log("new", newVal, "prev", prevVal);
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
