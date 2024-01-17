<script setup>
import { defineComponent, ref } from 'vue';
import {db} from '../firebase.js'
import { collection , addDoc , doc , updateDoc} from 'firebase/firestore'

const props = defineProps({
    data: Object,
    isNew: Boolean
})

defineComponent ({
   name: 'AddOrUpdate'
})

const emits = defineEmits(['close'])

const task = ref ({
    name: '',
    description: '',
})

async function AddOrUpdate() {
    if(props.isNew) {
       await addDoc(collection(db, 'tasks'), task.value).then((res) => {
        emits('close')
       })
    }
    else {

    }
}
</script>

<template>
   <transition name="modal">
   <div class="modal-overlay">
     <div class="modal-wrapper">
       <div class="modal-container">
           <input type="text" placeholder="Enter name" v-model="task.name" />
           <textarea placeholder="Enter description" v-model="task.description" />
           <button @click="emits('close')">Close</button>
           <button @click="AddOrUpdate()">{{isNew ? 'Add' : 'Update'}}</button>
           
       </div>
       </div>
     </div>
   </transition>
</template>

<style>
.modal-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vw;
    background: rgba(rgb(145, 14, 14));
    transition: opacity .3 ease;
}

.modal-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
}

.modal-container {
    background: #8e9099;
    width: 900px;
}

input , textarea {
    width: 90%;
    margin: auto;
    margin-bottom: 20px;
    overflow: hidden;
}

</style>