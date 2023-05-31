<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import Card from './Card.vue'
import Close from './Close.vue';
defineEmits(['finish'])

const props = defineProps<{ etudiant: string, isTiersTemps: boolean }>()
let addedAt = new Date()
let step = ref("")
let running = ref(false)
const timer: any = ref({
    "preparation": 1800,
    "entretien": 1200,
    "realisation": 3600,
    "recettage": 1200,
})

const endTime: any = ref({
    "preparation": "--:--",
    "entretien": "--:--",
    "realisation": "--:--",
    "recettage": "--:--",
})

if(props.isTiersTemps){
    for (const key in timer.value) {
        timer.value[key] = timer.value[key] + (timer.value[key] / 3)
    }
}

const addedAtFormated = computed(() => addedAt.toLocaleTimeString("fr-FR", {hour: '2-digit', minute:'2-digit', hour12: false}))

function toggleTimer(targetStep: string){
    if(step.value == targetStep && running.value){
        running.value = false
    } else {
        running.value = true
    }

    endTime.value[targetStep] = formatEndDate(timer.value[targetStep])

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
        s = Math.abs(s)
        return '-' + fancyTimeFormat(s)
    }

    return(s-(s%=60))/60+(9<s?':':':0')+s
}

function formatEndDate(s: number): string
{   
    let date = new Date()
    return date.toLocaleTimeString("fr-FR", {hour: '2-digit', minute:'2-digit', hour12: false})
}

</script>

<template>
    <h3 class="text-left dark:text-white font-bold text-xl">
        <Close @close="$emit('finish')" />
        À {{addedAtFormated}} : {{etudiant}}
    </h3>
    <div class="grid mb-8 gap-2 md:gap-2 grid-cols-4 md:grid-cols-4">
        <Card class="pointer" :targetTime="endTime.preparation" @click="toggleTimer('preparation')" :active="running && step === 'preparation'" title="Preparation" :value="fancyTimeFormat(timer.preparation)"></Card>
        <Card class="pointer" :targetTime="endTime.entretien" @click="toggleTimer('entretien')" :active="running && step === 'entretien'" title="Entretien" :value="fancyTimeFormat(timer.entretien)"></Card>
        <Card class="pointer" :targetTime="endTime.realisation" @click="toggleTimer('realisation')" :active="running && step === 'realisation'" title="Réalisation" :value="fancyTimeFormat(timer.realisation)"></Card>
        <Card class="pointer" :targetTime="endTime.recettage" @click="toggleTimer('recettage')" :active="running && step === 'recettage'" title="Recettage" :value="fancyTimeFormat(timer.recettage)"></Card>
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