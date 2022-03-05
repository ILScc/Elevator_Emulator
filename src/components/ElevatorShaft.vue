<template>
    <div class="elevator-shaft">
        <div v-for="(floor, i) in floors" :key="i" class="floor">
            <span class="floor__number">{{ floor }}</span>
            <Transition :name="currentMovement"
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
            prevFloor: null,
            currentMovement: "",
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
    computed: {
        transformLength() {
            const diff = this.currentFloor - this.prevFloor;
            return `${diff}00%`;
        },
    },
    watch: {
        destinationFloor(newFloor) {
            if (!newFloor) {
                return;
            }
            this.prevFloor = this.currentFloor;
            console.log("in watch", this.currentFloor, newFloor);
            //TODO: this only works in one direction
            this.currentFloor < newFloor
                ? (this.currentMovement = "up")
                : (this.currentMovement = "down");
            setTimeout(() => {
                this.currentFloor = newFloor;
                this.$emit("floor-set");
                console.log(this.currentFloor, this.prevFloor);
            }, 1000);
        },
    },
};
</script>

<style scoped>
.up-enter-active,
.down-enter-active {
    transition: all 1s ease-out;
}

.up-enter-from {
    transform: translateY(v-bind(transformLength));
}
.down-enter-from {
    transform: translateY(v-bind(transformLength));
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
