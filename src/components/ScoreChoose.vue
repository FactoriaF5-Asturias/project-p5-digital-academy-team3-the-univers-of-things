<script setup>
import { computed, ref } from 'vue';
import { Star } from 'lucide-vue-next';
import { useScoreStore } from '@/stores/score-store';

const props = defineProps([
    "id"
])

const availableRates = ref([
    1, 2, 3, 4, 5,
    6, 7, 8, 9, 10
])

const chosen = ref(null)
// const disabled = ref(false)
const hovered = ref(null)
const bouncing = ref(null)

const { addScore } = useScoreStore();

async function buttonHandler(score) {
    bouncing.value = score;
    if (chosen.value === score) {
        chosen.value = null;
        hovered.value = null;
        await addScore(props.id, null);
        return;
    }
    await addScore(props.id, score);
    chosen.value = score;
}

</script>

<template>
    <div
        class="stars_container"
        @mouseleave="hovered = null"
    >
        <div 
            v-for="n in availableRates" 
            :key="n" 
            @click="buttonHandler(n)"
            @mouseenter="hovered = n"
            class="star available"
        >
            <Star 
                size="18"
                stroke-width="2"
                :fill="n <= (hovered !== null ? hovered : (chosen ?? 0)) ? 'currentColor' : 'none'"
                :class="n === bouncing ? 'pop' : ''"
                @animationend="bouncing = null"
            /> 
        </div>
    </div> 
</template>

<style scoped>
@reference "../assets/main.css";

.stars_container {
    @apply 
        flex flex-row justify-between items-center
        w-full 
    ;
}

.star {
    @apply
        p-1 md:p-2 lg:p-1 text-text-brand
    ;
}

.available {
    @apply
        hover:text-text-default hover:scale-125
    ;
}

@keyframes pop {
    0% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.5);
    }
    100% {
        transform: scale(1);
    }
}

.pop {
    animation: pop 0.3s ease;
}

</style>