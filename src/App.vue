<script setup>
import { ref, onMounted, computed, watch } from 'vue';

const tasks = ref([]);
const name = ref('');
const new_task = ref('');
const input_category = ref('personal');
const todos_asc = computed(() => tasks.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
}));

const addTask = () => {
    if (new_task.value.trim() === '') {
        return;
    }
}

watch(input_category, (newCat) => {
    console.log(newCat);
});

onMounted(() => {
    name.value = localStorage.getItem('name') || '';
});

</script>

<template>
    <div class="max-w-lg mx-auto py-10">
        <div class="mx-4">
            <form @submit.prevent="addTask">
                <input
                    type="text"
                    placeholder="e.g. Do exercise"
                    class="w-full py-2 px-3 rounded-xl bg-gray-200 outline-none mb-4"
                    v-model="new_task"
                >

                <h4>Pick a category</h4>
                <div class="flex flex-row justify-center space-x-5 select-none mt-2">
                    <label for="cat1" class="flex flex-col items-center cursor-pointer rounded shadow-lg w-full py-3 ring-1 ring-gray-200 active:scale-95 space-y-1">
                        <input
                            type="radio"
                            name="category"
                            id="cat1"
                            value="business"
                            class="w-5 h-5"
                            v-model="input_category"
                        >
                        <span>Business</span>
                    </label>
                    <label for="cat2" class="flex flex-col items-center cursor-pointer rounded shadow-lg w-full py-3 ring-1 ring-gray-200 active:scale-95 space-y-1">
                        <input
                            type="radio"
                            name="category"
                            id="cat2"
                            value="personal"
                            class="w-5 h-5 accent-pink-500"
                            v-model="input_category"
                        >
                        <span>Personal</span>
                    </label>
                </div>
                <button type="submit" class="bg-red-400 w-full rounded-xl text-gray-100 py-2 mt-4">Add Task</button>
            </form>
        </div>
    </div>
</template>
