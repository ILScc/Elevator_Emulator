<template>
    <div class="container">
        <ElevatorShaft :floors="floors" />
        <ElevatorControls :floors="floors" />
    </div>
</template>

<script>
import ElevatorShaft from "./components/ElevatorShaft.vue";
import ElevatorControls from "./components/ElevatorControls.vue";
export default {
    componets: {
        ElevatorShaft,
    },
    data() {
        return {
            elevatorShafts: 1,
            floors: 5,
        };
    },
    components: { ElevatorShaft, ElevatorControls },
};
</script>

<style scoped>
.container {
    display: grid;
    width: 1200px;
    height: 600px;
    grid-template-columns: 1fr 2fr;
    grid-template-rows: 1fr;
    box-sizing: border-box;
}
</style>
