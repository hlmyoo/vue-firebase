<script setup>
import {db} from '../firebase.js'
import {getDocs, collection} from 'firebase/firestore'
import {ref, onMounted} from 'vue'
import AddOrUpdate from './AddOrUpdate.vue';

const tasks = ref([])

const show = ref(false)

const data = ref({})

const isNew = ref(true)

onMounted (async() => {
let tasksCollection = await getDocs(collection(db, 'tasks'))
tasksCollection.forEach((task) => {
  tasks.value.push({...task.data(), id: task.id})
})
})

function toggleComponent(docData = {}, isNewDoc = true) {
data.value = docData
isNew.value = isNewDoc
  show.value = !show.value
}
</script>

<template>
  <div>
    <AddOrUpdate @close="toggleComponent" v-if="show" :data="data" :isNew="isNew"></AddOrUpdate>
    <button @click="toggleComponent()">Add New</button>
    <div v-for="task in tasks" :key="task.id">
      <div>Name: {{task.name}}</div>
      <div>Description: {{task.description}}</div>
    </div>
  </div>
</template>
