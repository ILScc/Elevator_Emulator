<template>
    <div class="container">
        <ElevatorShaft
            v-for="shaft in elevatorShafts"
            :key="shaft"
            :floors="floors"
        />
        <ElevatorControls :floors="floors" />
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
        };
    },
};
</script>

<style scoped>
.container {
    display: grid;
    width: 1200px;
    height: 600px;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    grid-template-rows: 1fr;
    box-sizing: border-box;
}
</style>
