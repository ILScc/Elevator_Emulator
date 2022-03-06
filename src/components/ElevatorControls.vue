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
    appearance: none;
    background-color: #fff;
    border-radius: 24px;
    border-style: none;
    box-shadow: rgba(0, 0, 0, 0.2) 0 3px 5px -1px,
        rgba(0, 0, 0, 0.14) 0 6px 10px 0, rgba(0, 0, 0, 0.12) 0 1px 18px 0;
    color: #3c4043;
    cursor: pointer;
    font-size: 14px;
    height: 48px;
    justify-content: center;
    letter-spacing: 0.25px;
    max-width: 100%;
    padding: 2px 24px;
    position: relative;
    text-align: center;
    transition: box-shadow 280ms cubic-bezier(0.4, 0, 0.2, 1),
        opacity 15ms linear 30ms, transform 270ms cubic-bezier(0, 0, 0.2, 1) 0ms;
    z-index: 0;
}
.elevator-controls__btn:hover {
    background: #f6f9fe;
    color: #174ea6;
}
.elevator-controls__btn_rejected {
    animation: headShake 1s ease-in-out;
    border: 1px solid red;
    position: relative;
}
.elevator-controls__btn_rejected::after {
    content: attr(data-reason);
    display: block;
    position: absolute;
    margin-left: auto;
    margin-right: auto;
    left: 0;
    right: 0;
    top: -50%;
}
.elevator-controls__container {
    margin-left: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
}
@keyframes headShake {
    0% {
        transform: translateX(0);
    }

    6.5% {
        transform: translateX(-6px) rotateY(-9deg);
    }

    18.5% {
        transform: translateX(5px) rotateY(7deg);
    }

    31.5% {
        transform: translateX(-3px) rotateY(-5deg);
    }

    43.5% {
        transform: translateX(2px) rotateY(3deg);
    }

    50% {
        transform: translateX(0);
    }
}
</style>
