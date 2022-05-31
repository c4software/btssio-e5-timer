<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import Card from './Card.vue'
defineProps<{ etudiant: string }>()

let addedAt = new Date()
let step = ref("")
let running = ref(false)
const timer: any = ref({
    "preparation": 1800,
    "realisation": 3600,
    "Recettage": 1200,
})

const hasRemainingTime = computed(() => timer.value[step.value] > 0)
const addedAtFormated = computed(() => addedAt.toLocaleTimeString("fr-FR", {hour: '2-digit', minute:'2-digit', hour12: false}))
const buttonName = computed(() => {
    return running.value ? 'Stop' : 'Demarrer'
})

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
    <h3 class="text-left">À {{addedAtFormated}} : {{etudiant}}</h3>
    <div class="grid mb-8 gap-3 md:gap-6 grid-cols-3 md:grid-cols-3">
        <Card class="pointer" @click="toggleTimer('preparation')" :active="running && step === 'preparation'" title="Preparation" :value="fancyTimeFormat(timer.preparation)"></Card>
        <Card class="pointer" @click="toggleTimer('realisation')" :active="running && step === 'realisation'" title="Réalisation" :value="fancyTimeFormat(timer.realisation)"></Card>
        <Card class="pointer" @click="toggleTimer('presentation')" :active="running && step === 'presentation'" title="Presentation" :value="fancyTimeFormat(timer.presentation)"></Card>
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