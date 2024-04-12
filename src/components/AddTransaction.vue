<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter text...">
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
            amount: parseFloat(amount.value)
        }

        emit('transactionSubmitted', transactionData)

        text.value = '';
        amount.value = '';
    };
</script>