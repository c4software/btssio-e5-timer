<template>
    <button @click="$emit('close')" type="button" class="bg-white dark:bg-gray-800 rounded-md p-2 my-2 inline-flex items-center justify-center text-gray-400 hover:text-white hover:bg-red-500 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-indigo-500">
        <svg class="h-3 w-3" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
    </button>
</template>

<script lang="ts" setup>
defineEmits(['close'])
</script>