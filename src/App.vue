<script setup>
import { data } from 'autoprefixer';
import {ref} from 'vue'

  const datas = ref('')
  const defaultData = [{
				myList: 'Laundry  '
			}]
  const todoData = JSON.parse(localStorage.getItem('todo')) || defaultData
  const obj = ref(todoData)  
  

  function addItem(){
    if(datas.value != ''){
      const item = {myList: datas.value}
      obj.value.push(item)
      datas.value = ''
      saveLocalStorage()
    }
  }

  function removeItem(item){
     obj.value.splice(item,1)
     saveLocalStorage()
  }

  function saveLocalStorage(){
    const storageData = JSON.stringify(obj.value);
    localStorage.setItem('todo', storageData)
  }
</script>

<template>
  <header>
    
  </header>

  <main class="bg-violet-400 h-screen">
    <div class="flex justify-center items-center h-full">
      <div class="w-fit flex flex-col gap-3">
        <h1 class="text-3xl font-bold mb-4 text-white" >Welcome to TODO list </h1>
        <div class="flex gap-2">
          <input class="p-2 rounded-lg flex-1" type="text" v-model="datas">
          <button class="p-2 bg-yellow-100 rounded-lg" @click="addItem">Click Me</button>
        </div>
        <!-- Todo List -->
        <div class="p-0 font-bold">Todo-List</div>
        <hr>
        <div v-for="item, index in obj" :key="index" class="p-3 bg-orange-300 rounded-lg flex justify-between text-neutral-600"> {{item.myList}}
          <button class="text-sm text-red-500 font-thin bg-white rounded-md p-2" @click="removeItem(index)">Remove</button>
        </div>
      </div>
        
    </div>
    
  </main>
</template>

