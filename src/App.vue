<script setup>
import modal from './components/modal.vue'
import { ref } from 'vue'

const datas = ref('')
const showModal = ref(false)
const modalValue = ref([])
const todoData = JSON.parse(localStorage.getItem('todo')) || []
const obj = ref(todoData)

function showModals(item){
  showModal.value = true
  modalValue.value = item
}

function addItem() {
  
  if (datas.value != '') {
    const item = { 
      id: todoData.length + 1,
      myList: datas.value 
    }
    obj.value.push(item)
    datas.value = ''
    saveLocalStorage()
  }
}

function removeItem(item) {
  const index = item - 1
  obj.value.splice(index, 1)
  saveLocalStorage()
}

function updateItem(id, item){
  const index = id - 1
  const newObj = {
    id : id,  
    myList: item
  }
  obj.value.splice(index, 1, newObj )
  saveLocalStorage()
}

function saveLocalStorage() {
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
        <h1 class="text-3xl font-bold mb-4 text-white">Welcome to TODO list </h1>
        <div class="flex gap-2">
          <input class="p-2 rounded-lg flex-1" type="text" v-model="datas">
          <button class="p-2 bg-yellow-100 rounded-lg" @click="addItem">Click Me</button>
        </div>
        <!-- Todo List -->
        <div class="p-0 font-bold">Todo-List</div>
        <hr>
        <div v-for="item in obj" :key="item.id"
          class="p-3 bg-orange-300 rounded-lg flex justify-between text-neutral-600">
          {{ item.myList }}

          <div class="flex gap-2">

            <button @click="showModals(item)" class="p-2 bg-white text-green-500 rounded">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-4">
                <path stroke-linecap="round" stroke-linejoin="round"
                  d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
              </svg>
              
            </button>
            <button class="text-sm text-red-500 font-thin bg-white rounded-md p-2" @click="removeItem(item.id)"><svg
                xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="size-4">
                <path stroke-linecap="round" stroke-linejoin="round"
                  d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
              </svg>
            </button>

            
          </div> 
        </div>

      </div>

    </div>

    <modal
      :myModal = showModal
      @closeModal = "showModal = false"
      :modalValue = modalValue
      @send-my-list = "updateItem"
    />
  </main>
</template>
