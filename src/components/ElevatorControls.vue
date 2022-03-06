<template>
    <div class="elevator-controls">
        <div
            v-for="(floor, i) in floors"
            :key="i"
            class="elevator-controls__container"
        >
            <button
                @click="$emit('elevator-called', floor)"
                class="elevator-controls__btn"
                :class="{
                    'elevator-controls__btn_rejected':
                        callStatus.calledFloor === floor &&
                        !callStatus.accepted,
                }"
                :data-reason="`${callStatus.reason}`"
            >
                Call elevator on floor {{ floor }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        floors: {
            type: Number,
            required: true,
        },
        callStatus: {
            type: Object,
            required: true,
        },
    },
    emits: {
        "elevator-called": null,
    },
};
</script>

<style>
.elevator-controls {
    display: flex;
    flex-direction: column-reverse;
    border: 1px solid tomato;
}
.elevator-controls__btn {
    width: 30%;
    height: 30%;
}
.elevator-controls__container {
    margin-left: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
}
</style>
