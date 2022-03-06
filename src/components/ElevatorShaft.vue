<template>
    <div class="elevator-shaft">
        <div v-for="(floor, i) in floors" :key="i" class="floor">
            <span class="floor__number">{{ floor }}</span>
            <Transition :name="currentMovement"
                ><AppElevator
                    :class="{ waiting: isWaiting }"
                    :style="{
                        '--length': floorsOffset,
                        '--move-time': moveTime,
                        '--wait-time': `${$options.ELEVATOR_WAITING_TIME}ms`,
                    }"
                    :destination="destinationFloor"
                    :currentFloor="currentFloor"
                    :prevFloor="prevFloor"
                    v-if="currentFloor === floor"
            /></Transition>
        </div>
    </div>
</template>

<script>
import AppElevator from "./AppElevator.vue";
export default {
    data() {
        return {
            prevFloor: 1,
            currentFloor: 1,
            currentMovement: "",
            floorsOffset: "",
            moveTime: "",
            isWaiting: false,
        };
    },
    ELEVATOR_WAITING_TIME: 3000,
    components: { AppElevator },
    props: {
        floors: {
            type: Number,
            required: true,
        },
        destinationFloor: {
            type: Number,
            required: false,
        },
    },
    emits: {
        "floor-set": null,
    },
    methods: {
        async elevatorWorking() {
            const movingTime = Number.parseInt(this.moveTime);
            await new Promise((resolve) => setTimeout(resolve, movingTime));
            await Promise.resolve((this.isWaiting = true));
            await new Promise((resolve) =>
                setTimeout(resolve, this.$options.ELEVATOR_WAITING_TIME)
            );
            await Promise.resolve((this.isWaiting = false));
            this.$emit("floor-set");
        },
    },
    watch: {
        destinationFloor(destination) {
            if (!destination) {
                return;
            }
            this.prevFloor = this.currentFloor;
            const offset = destination - this.currentFloor;
            this.floorsOffset = `${offset}00%`;
            this.moveTime = `${Math.abs(offset * 1000)}ms`;
            this.currentFloor < destination
                ? (this.currentMovement = "up")
                : (this.currentMovement = "down");
            this.currentFloor = destination;
            this.elevatorWorking();
        },
    },
};
</script>

<style scoped>
.up-enter-active,
.down-enter-active {
    transition: all var(--move-time) ease-out;
}

.up-enter-from {
    transform: translateY(var(--length));
}

.down-enter-from {
    transform: translateY(var(--length));
}
.waiting {
    animation: flash var(--wait-time);
}
@keyframes flash {
    from,
    50%,
    to {
        opacity: 1;
    }

    25%,
    75% {
        opacity: 0;
    }
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
    align-items: center;
    position: relative;
}
.floor__number {
    position: absolute;
    top: 15px;
    left: 15px;
}
.floor:not(:last-child)::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    width: 100%;
    border: 1px dashed blue;
}
</style>
