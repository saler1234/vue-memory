<template>
    <div @click="selectCard" class="card" :class="flippedStyles">
        <div class="card-face is-front">
            <img :src="`/images/${value}.png`" :alt="value">
            <img v-if="matched" src="../../public/images/checkmark.svg" class="icon-checkmark" />
        </div>
        <div class="card-face is-back"></div>
    </div>
</template>

<script>
import { computed } from '@vue/runtime-core'
export default {
    props: {
        matched: {
            type: Boolean,
            default: false,
        },
        position: {
            type: Number,
            required: true
        },
        value: {
            type: String,
            required: true
        },
        visible: {
            type: Boolean,
            default: false,
        }
    },
    setup(props, context) {
        const flippedStyles = computed(() => {
            if (props.visible) {
                return 'is-flipped'
            }
        })

        const selectCard = () => {
            context.emit('select-card', {
                position: props.position,
                faceValue: props.value
            })
        }
        
        return { selectCard, flippedStyles }
    }
}
</script>

<style>
.card {
    position: relative;
    transition: 0.5s transform ease-in;
    transform-style: preserve-3d;
}
.card.is-flipped {
    transform: rotateY(180deg)
}
.card-face {
    height: 100%;
    width: 100%;
    position: absolute;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    backface-visibility: hidden;
}
.card-face.is-front {
    background-image: url('/public/images/card-bg.png');
    color: white;
    transform: rotateY(180deg)
}
.card-face.is-back {
    background-image: url('/public/images/card-bg-empty.png');
    color: white;
}
.icon-checkmark {
    position: absolute;
    right: 5px;
    bottom: 5px;
}
</style>