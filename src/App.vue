<template>
    <div class="min-h-screen flex items-center justify-center bg-gray-100">
        <div class="bg-white p-6 rounded-lg shadow-lg w-full max-w-lg">
            <h1 class="text-2xl font-bold mb-4">Date Difference Calculator</h1>

            <div class="mb-4">
                <DatePicker v-model="startDate" placeholder="Select start date"/>
            </div>

            <div class="mb-4">
                <DatePicker v-model="endDate" placeholder="Select end date"/>
            </div>

            <label class="flex items-center mb-4">
                <input type="checkbox" v-model="includeEndDate" class="form-checkbox"/>
                <span class="ml-2">Include end date</span>
            </label>

            <button @click="calculateDifference" class="bg-blue-500 text-white py-2 px-4 rounded">
                Calculate
            </button>

            <div v-if="result" class="mt-6">
                <h2 class="text-xl font-semibold">Results</h2>
                <p>{{ result }}</p>
            </div>
        </div>
    </div>
</template>

<script setup>
import {ref} from 'vue';
import DatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';

const startDate = ref(null);
const endDate = ref(null);
const includeEndDate = ref(false);
const result = ref(null);

const calculateDifference = () => {
    if (!startDate.value || !endDate.value) {
        result.value = 'Please select both dates.';
        return;
    }

    const start = new Date(startDate.value);
    const end = new Date(endDate.value);

    if (includeEndDate.value) {
        end.setDate(end.getDate() + 1);
    }

    const diff = Math.abs(end - start);
    const years = Math.floor(diff / (1000 * 60 * 60 * 24 * 365));
    const months = Math.floor((diff % (1000 * 60 * 60 * 24 * 365)) / (1000 * 60 * 60 * 24 * 30));
    const weeks = Math.floor((diff % (1000 * 60 * 60 * 24 * 30)) / (1000 * 60 * 60 * 24 * 7));
    const days = Math.floor((diff % (1000 * 60 * 60 * 24 * 7)) / (1000 * 60 * 60 * 24));
    const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
    const seconds = Math.floor((diff % (1000 * 60)) / 1000);

    result.value = `
    ${years} years, ${months} months, ${weeks} weeks, ${days} days,
    ${hours} hours, ${minutes} minutes, ${seconds} seconds
  `;
};
</script>

