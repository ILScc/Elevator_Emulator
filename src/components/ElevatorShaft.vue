<template>
    <div class="elevator-shaft">
        <div v-for="(floor, i) in floors" :key="i" class="floor">
            <span class="floor__number">{{ floor }}</span>
            <Transition name="movement"
                ><AppElevator
                    :class="{ waiting: isWaiting }"
                    :style="{
                        '--floors-to-go': `${floorsOffset}00%`,
                        '--move-time': `${Math.abs(floorsOffset * 1000)}ms`,
                        '--wait-time': `${$options.ELEVATOR_WAITING_TIME}ms`,
                    }"
                    :destination="destinationFloor"
                    :currentFloor="currentFloor"
                    :prevFloor="prevFloor"
                    :isReady="isReady"
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
            currentMovement: "",
            floorsOffset: 0,
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
        shaftId: {
            type: Number,
            required: true,
        },
        currentFloor: {
            type: Number,
            required: true,
        },
        isReady: {
            type: Boolean,
            required: true,
        },
    },
    emits: {
        "elevator-ready": null,
        "elevator-landed": null,
    },
    methods: {
        async elevatorWorking(destination) {
            const movingTime = Math.abs(this.floorsOffset * 1000);

            this.$emit("elevator-landed", [this.shaftId, destination]);

            await new Promise((resolve) => setTimeout(resolve, movingTime));
            await Promise.resolve((this.isWaiting = true));
            await new Promise((resolve) =>
                setTimeout(resolve, this.$options.ELEVATOR_WAITING_TIME)
            );
            await Promise.resolve((this.isWaiting = false));
            this.$emit("elevator-ready", [this.shaftId, destination]);
        },
    },
    watch: {
        destinationFloor(destination) {
            if (!destination) {
                return;
            }
            this.floorsOffset = destination - this.currentFloor;
            this.prevFloor = this.currentFloor;

            this.elevatorWorking(destination);
        },
    },
};
</script>

<style scoped>
.movement-enter-active {
    transition: all var(--move-time) ease-out;
}

.movement-enter-from {
    transform: translateY(var(--floors-to-go));
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
