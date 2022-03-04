<template>
    <div class="elevator-shaft">
        <div v-for="(floor, i) in floors" :key="i" class="floor">
            {{ floor }}
            <Transition
                ><AppElevator v-if="currentFloor === floor"
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
                this.$emit("floor-set");
                return;
            }
            this.currentFloor = newFloor;
            this.$emit("floor-set");
        },
    },
};
</script>

<style scoped>
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
    justify-content: center;
    align-items: center;
    position: relative;
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
