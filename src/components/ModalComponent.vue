<template>
    <div v-if="showModal" class="modal-overlay" @click.self="closeModal">
      <div class="modal">
        <h2>Transaction Details</h2>
        <p><strong>Text:</strong> {{ transaction.text }}</p>
        <p><strong>Description:</strong> {{ transaction.description }}</p>
        <p><strong>Category:</strong> {{ transaction.category }}</p>
        <p><strong>Amount:</strong> ${{ transaction.amount }}</p>
        <button @click="closeModal">Close</button>
      </div>
    </div>
  </template>
  
<script setup>
import { defineProps } from 'vue';

const props = defineProps({
transaction: Object,
showModal: Boolean
});

const emit = defineEmits(['close']);

const closeModal = () => {
    emit('close');
};
</script>

<style scoped>
.modal-overlay {
position: fixed;
top: 0;
left: 0;
right: 0;
bottom: 0;
background-color: rgba(0, 0, 0, 0.5);
display: flex;
align-items: center;
justify-content: center;
}

.modal {
background: white;
padding: 20px;
border-radius: 5px;
width: 300px;
box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

</style>
