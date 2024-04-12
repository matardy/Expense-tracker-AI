<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter text...">
        </div>
        <div class="form-control">
            <label for="description">Description</label>
            <textarea id="description" v-model="description" placeholder="Enter a short description..."  maxlength="150"></textarea>
        </div>
        <div class="form-control">
            <label for="category">Category</label>
            <select id="category" v-model="category">
                <option value="food">Food</option>
                <option value="pleasure">Pleasure</option>
                <option value="education">Education</option>
                <option value="transportation">Transportation</option>
                <option value="gifts">Gifts</option>
            </select>
        </div>


        <div class="form-control">
            <label for="amount" >Amount <br /> 
            (negative - expense, positive - income)</label>
            <input type="number" id="amount" v-model.number="amount" placeholder="Enter amount...">
        </div>
        <button class="btn">Add transaction</button>

    </form>
</template>

<script setup>
import {ref} from 'vue'; 
import {useToast} from 'vue-toastification';

const text = ref('');
const description = ref(''); 
const category = ref(''); 
const amount = ref('');

const emit = defineEmits(['transactionSubmitted']);

const toast = useToast();

const onSubmit = () => {
    if (!text.value.trim()){
        toast.error('Text field mus be filled');
        return;
    }

    // Validate if amount is a number and not NaN
    if (!amount.value || isNaN(parseFloat(amount.value))) {
        toast.error('Please enter a valid number for the amount');
        return;
    }
    
    const transactionData = {
        text: text.value, 
        description: description.value,
        category: category.value,
        amount: parseFloat(amount.value)
    }

    emit('transactionSubmitted', transactionData)

    text.value = '';
    description.value = '';
    category.value = '';
    amount.value = '';
};
</script>

<style scoped>
.form-control textarea,
.form-control select {
  width: 100%;
  padding: 10px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

.form-control select {
  height: 34px;
  background-color: white;
}

.form-control textarea {
  height: 100px;
  resize: vertical; /* Allows the user to vertically resize the textarea (not horizontally) */
}

</style>