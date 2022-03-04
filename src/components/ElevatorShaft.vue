<template>
    <div class="elevator-shaft">
        <div v-for="(floor, i) in floors" :key="i" class="floor">
            <span class="floor__number">{{ floor }}</span>
            <Transition name="elevator"
                ><AppElevator
                    :destination="destinationFloor"
                    :currentFloor="currentFloor"
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
            currentFloor: 1,
        };
    },
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

    watch: {
        destinationFloor(newFloor, oldFloor) {
            if (!newFloor) {
                return;
            }
            if (oldFloor === newFloor) {
                return;
            }
            setTimeout(() => {
                this.currentFloor = newFloor;
                this.$emit("floor-set");
            }, 1000);
        },
    },
};
</script>

<style scoped>
.elevator-enter-active {
    transition: all 0.3s ease-out;
}

.elevator-leave-active {
    transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.elevator-enter-from,
.elevator-leave-to {
    transform: translateX(20px);
    opacity: 0;
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
