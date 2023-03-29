<template>
  <main class="h-screen w-screen">
    <!-- overlay -->
    <div 
    v-if="overlay"
    class="h-screen absolute w-screen bg-[rgba(0,0,0,0.77)] flex items-center justify-between">
      <div class="w-1/2 rounded-lg relative flex flex-col mx-auto">
        <textarea  v-model.trim="textArea" name="note" id="note" cols="30" rows="10"></textarea>
        <p 
        class="text-red-500"
        v-if="errorMessage"
        >{{ errorMessage }}</p>
        <button 
        @click="textAreaValue"
        class="bg-blue-500 border-none py-3 px-5 mt-3">Add Note</button>
        <button 
        @click="overLayToggle"
        class="bg-red-700 mt-2"> close </button>
      </div>
    </div>

    <div class="max-w-7xl mx-auto px-12">
      <header class="flex justify-between items-center mb-10">
        <h1 class="text-4xl font-bold">Notes</h1>
        <button 
        @click="overLayToggle"
        class="border-none w-8 h-8 text-white bg-black rounded-full">+</button>
      </header>

      <!-- card -->
      <div class="flex">
        <div 
        v-for="data in notes" :key="data.id"
        :style="{backgroundColor: data.color}"
        class="w-64 h-64 p-2 rounded-lg flex flex-col justify-between mr-2 mb-2">
          <p class="text-md">{{ data.text }}</p>
          <p class="text-base">{{ data.date.toLocaleDateString("en-us") }}</p>
        </div>      
      </div>
    </div>
  </main>
</template>

<script setup>

  import { ref } from "vue";
  const overlay = ref(false);

  const overLayToggle = () => {
    overlay.value = !overlay.value;
  }

  const textArea = ref();
  const cardShow = ref(false)
  const notes = ref([])
  const errorMessage = ref()
  const textAreaValue = () => { 
    if(textArea.value.length <= 10){
      errorMessage.value = "Note needs to be 10 characters or more"
      return
    }else{
      errorMessage.value = ""
      notes.value.push({
      id: Math.floor(Math.random()*2000),
      text: textArea.value,
      date: new Date(),
      color: "hsl(" + Math.random() * 360 + ", 100%, 75%)",
  })
    textArea.value = "";
    overlay.value = false;
    }
  }
</script>