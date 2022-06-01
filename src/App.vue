<script setup lang="ts">
import { ref } from 'vue'
import Timers from './components/Timers.vue';

const etudiants = ref({} as { [key: string]: string; });

let studentNameInput = ref("");

function addStudent(){
  if(studentNameInput.value){
    etudiants.value[Date.now()] = studentNameInput.value; 
    studentNameInput.value = '';
  }
}

function removeStudent(key: any){
  delete etudiants.value[key];
}

</script>

<template>
  <div class="text-center">
    <img alt="BTS SIO" class="logo mx-auto m-10" src="./assets/logo-certa.png" />
  </div>

  <div class="py-5">
    <main class="h-full overflow-y-auto">
        <div class="container  mx-auto grid">
          <Timers @finish="() => removeStudent(key)" :key="key" v-for="(value, key) in etudiants" :etudiant="value" />
        </div>
    </main>

    <div class="text-center p-10">
      <input @keyup.enter="addStudent" class="pl-2 border rounded py-2 px-3" style="margin-right: 10px; margin-bottom: 10px;" type="text" placeholder="Nom étudiant" v-model="studentNameInput"/>
      <button type="button" @click="addStudent" :disabled="!studentNameInput" :class="{'opacity-50': !studentNameInput, 'cursor-not-allowed': !studentNameInput}" class="focus:outline-none text-white bg-green-700 hover:bg-green-800 focus:ring-4 focus:ring-green-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800">Ajouter un étudiant</button>
    </div>
  </div>

</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.logo{
  height: 100px;
}
</style>
