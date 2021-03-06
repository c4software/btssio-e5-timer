<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import Card from './Card.vue'
import Close from './Close.vue';
defineProps<{ etudiant: string }>()
defineEmits(['finish'])

let addedAt = new Date()
let step = ref("")
let running = ref(false)
const timer: any = ref({
    "preparation": 1800,
    "entretien": 1200,
    "realisation": 3600,
    "recettage": 1200,
})

const addedAtFormated = computed(() => addedAt.toLocaleTimeString("fr-FR", {hour: '2-digit', minute:'2-digit', hour12: false}))
const btnClass = computed(() => running.value ? 'fixed-width px-3 py-2 text-xs font-medium text-center text-white bg-red-700 rounded-lg hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800' : 'fixed-width px-3 py-2 text-xs font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800')

function toggleTimer(targetStep: string){
    if(step.value == targetStep && running.value){
        running.value = false
    } else {
        running.value = true
    }

    step.value = targetStep
}

const timeInterval = ref();
watch(running, (value) => {
    if(!value){
        clearInterval(timeInterval.value)
    } else {
        timeInterval.value = setInterval(() => {
            timer.value[step.value]--;
        }, 1000)
    }
})

function fancyTimeFormat(s: number): string
{   
    if(s < 0){
        return s.toString();
    }

    return(s-(s%=60))/60+(9<s?':':':0')+s
}

</script>

<template>
    <h3 class="text-left dark:text-white font-bold text-xl">
        <Close @close="$emit('finish')" />
        À {{addedAtFormated}} : {{etudiant}}
    </h3>
    <div class="grid mb-8 gap-2 md:gap-2 grid-cols-4 md:grid-cols-4">
        <Card class="pointer" @click="toggleTimer('preparation')" :active="running && step === 'preparation'" title="Preparation" :value="fancyTimeFormat(timer.preparation)"></Card>
        <Card class="pointer" @click="toggleTimer('entretien')" :active="running && step === 'entretien'" title="Entretien" :value="fancyTimeFormat(timer.entretien)"></Card>
        <Card class="pointer" @click="toggleTimer('realisation')" :active="running && step === 'realisation'" title="Réalisation" :value="fancyTimeFormat(timer.realisation)"></Card>
        <Card class="pointer" @click="toggleTimer('recettage')" :active="running && step === 'recettage'" title="Recettage" :value="fancyTimeFormat(timer.recettage)"></Card>
    </div>
</template>

<style scoped>

.fixed-width {
    width: 100px;
}

.pointer{
    cursor: pointer;
}

</style>