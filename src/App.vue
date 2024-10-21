<script setup>
    import { ref } from 'vue';

    const tailwindLightColors = [
    'bg-red-200',
    'bg-orange-200',
    'bg-yellow-200',
    'bg-green-200',
    'bg-emerald-200',
    'bg-teal-200',
    'bg-blue-200',
    'bg-lightBlue-200',
    'bg-indigo-200',
    'bg-purple-200',
    'bg-pink-200',
    'bg-rose-200',
    'bg-gray-200',
    'bg-slate-200',
    'bg-zinc-200',
];
function getRandomColor() {
    const randomIndex = Math.floor(Math.random() * tailwindLightColors.length);
    return tailwindLightColors[randomIndex];
}
const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMsg = ref("");
const addNote = ()=>
{
    if(newNote.value.length<10)
    {
        return errorMsg.value = "Not less than 10 Character";
    }

    notes.value.push
    ({
        id: Date.now() + '-' + Math.floor(Math.random() * 10000),
        text : newNote.value,
        date: new Date(),
        bgColor: getRandomColor(),
    })
    showModal.value= false;
    newNote.value="";
    errorMsg.value="";
};
</script>


<template>
  
<main class="h-full w-full bg-gray-50">
    <div id="modal" v-show="showModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
        <div class="bg-white rounded-lg p-6 w-11/12 max-w-md">
            <h2 class="text-xl font-bold mb-4">Add Note </h2>
            <textarea id="noteContent" v-model="newNote" class="w-full border border-gray-300 rounded-lg p-2" rows="4"></textarea>
            <p 
            v-if="errorMsg" 
            class="text-red-500">{{ errorMsg }}
            </p>
            <div class="flex justify-end mt-4">
                <button 
                @click="addNote" 
                class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition duration-200">Save</button>
                <button 
                @click="(showModal=false, newNote= '')" 
                class="px-4 py-2 bg-red-500 text-white rounded-lg ml-2 hover:bg-gray-400 transition duration-200">Cancel</button>
            </div>
        </div>
    </div>


    <div class="container mx-auto p-6 md:p-10">
        <header class="flex justify-between items-center mb-6">
            <h1 class="font-bold text-3xl text-gray-800">Notes</h1>
            <button 
            @click="showModal=true" 
            class="px-3 py-2 bg-slate-700 text-xl shadow-md border-none text-white rounded-full hover:bg-black transition duration-200">
                +
            </button>
        </header>
            <div class="flex flex-wrap gap-6">
                <div 
                v-if="(notes.length==0 && showModal!=true)" 
                class="bg-gray-200 flex flex-col shadow-md rounded-lg p-4 border border-gray-200 flex-1 min-w-[280px] md:min-w-[300px]">
                    <h1 class="text-center">Tap on <span class="text-xl font-bold "> + </span> to add note...</h1>
                </div>
            <div 
                v-for="note in notes" 
                :key="note.id" 
                :class="['flex flex-col shadow-md rounded-lg p-4 border border-gray-200 flex-1 min-w-[280px] md:min-w-[300px]',note.bgColor ]">
                <p class="text-gray-700 text-lg">{{ note.text }}</p>
                <p class="text-gray-700 pt-4 text-sm">{{ note.date.toLocaleDateString('en-UK') }}</p>
            </div>
        </div>
    </div>
</main>

</template>