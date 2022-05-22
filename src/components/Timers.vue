<script setup lang="ts">
import { ref, computed, watch } from 'vue'
defineProps<{ etudiant: string }>()

let step = ref("preparation")
let running = ref(false)
const timer: any = ref({
    "preparation": 1800,
    "realisation": 3600,
    "presentation": 1800,
})

const hasRemainingTime = computed(() => timer.value[step.value] > 0)

const buttonName = computed(() => {
    return running.value ? 'Stop' : 'Demarrer'
})

const btnClass = computed(() => running.value ? 'fixed-width px-3 py-2 text-xs font-medium text-center text-white bg-red-700 rounded-lg hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800' : 'fixed-width px-3 py-2 text-xs font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800')

function toggleTimer(){
    running.value = !running.value
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

function fancyTimeFormat(s: number)
{   
    if(s < 0){
        return s;
    }

    return(s-(s%=60))/60+(9<s?':':':0')+s

}

</script>

<template>

    <tr>
        <td>{{etudiant}}</td>
        <td @click="step = 'preparation'" :class="step === 'preparation' ? 'font-bold' : 'pointer'"><div class="text-xl">{{fancyTimeFormat(timer.preparation)}}</div></td>
        <td @click="step = 'realisation'" :class="step === 'realisation' ? 'font-bold' : 'pointer'"><div class="text-xl">{{fancyTimeFormat(timer.realisation)}}</div></td>
        <td @click="step = 'presentation'" :class="step === 'presentation' ? 'font-bold' : 'pointer'"><div class="text-xl">{{fancyTimeFormat(timer.presentation)}}</div></td>
        <td>
            <button @click="toggleTimer" type="button" :class="btnClass">{{buttonName}}</button>
        </td>
    </tr>

</template>

<style scoped>

.fixed-width {
    width: 100px;
}

.pointer{
    cursor: pointer;
}

</style>