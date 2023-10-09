<script setup>
import { ref, onMounted, computed, watch } from 'vue';

const tasks = ref([]);
const new_task = ref('');
const input_category = ref('personal');

const addTask = () => {
    if (new_task.value.trim() === '') {
        return
    }

    tasks.value.push({
        content: new_task.value,
        category: input_category.value,
        done: false,
        created_at: new Date().getTime()
    });

    new_task.value = '';
};

const removeTask = task => {
    tasks.value = tasks.value.filter(t => t !== task)
}

watch(tasks, newVal => {
    localStorage.setItem('tasks', JSON.stringify(newVal));
}, { deep: true });

onMounted(() => {
    tasks.value = JSON.parse(localStorage.getItem('tasks')) || [];
});
</script>

<template>
    <div class="max-w-lg mx-auto py-10">
        <div class="mx-4">
            <form @submit.prevent="addTask" class="space-y-4">
                <input
                    type="text"
                    placeholder="e.g. Do exercise"
                    class="w-full py-2 px-3 rounded-lg outline-none mb-4 shadow-lg ring-1 ring-gray-200"
                    v-model="new_task">
                <div class="flex flex-row justify-center space-x-5 select-none">
                    <label for="cat1" class="flex flex-col items-center cursor-pointer rounded-lg shadow-lg w-full py-3 ring-1 ring-gray-200 active:scale-95 space-y-1 bg-white">
                        <input
                            type="radio"
                            name="category"
                            id="cat1"
                            value="business"
                            class="w-5 h-5"
                            v-model="input_category">
                        <span>Business</span>
                    </label>
                    <label for="cat2" class="flex flex-col items-center cursor-pointer rounded-lg shadow-lg w-full py-3 ring-1 ring-gray-200 active:scale-95 space-y-1 bg-white">
                        <input
                            type="radio"
                            name="category"
                            id="cat2"
                            value="personal"
                            class="w-5 h-5 accent-pink-500"
                            v-model="input_category">
                        <span>Personal</span>
                    </label>
                </div>
                <button type="submit" class="bg-pink-500 hover:bg-pink-400 transition duration-150 w-full rounded-xl text-gray-100 py-2">Add Task</button>
            </form>

            <div v-if="! tasks.length" class="mt-10 flex justify-center">You're done!</div>
            <div
                class="mt-4 space-y-4 text-gray-600"
                v-else>
                <h4 class="text-sm">Tasks:</h4>

                <div
                    v-for="(task, index) in tasks"
                    class="rounded-lg shadow-lg bg-white w-full py-2 px-4 flex items-center justify-between"
                    :class="[(task.done) ? 'line-through text-gray-400' : '']"
                    :key="index">
                    <div class="flex items-center grow">
                        <input
                            type="checkbox"
                            v-model="task.done"
                            class="appearance-none w-4 h-4 border rounded-full bg-white checked:border-white checked:ring-1 mr-2 cursor-pointer"
                            :class="[task.category === 'business' ? 'border-blue-500 checked:bg-blue-500 checked:ring-blue-500' : 'border-pink-500 checked:bg-pink-500 checked:ring-pink-500']">
                        <input type="text" v-model="task.content" class="outline-none grow cursor-pointer" spellcheck="false">
                    </div>

                    <button
                        class="text-red-700 rounded-lg hover:bg-gray-100 transition duration-150 p-2"
                        @click="removeTask(task)">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-4 h-4">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0" />
                        </svg>
                    </button>
                </div>

            </div>
        </div>
    </div>
</template>
