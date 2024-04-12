<template>
    <h3>History</h3>
    <div class="filter-control">
        <label for="filter">Filter by category:</label>
            <select id="filter" v-model="selectedCategory">
                <option value="">All Categories</option>
                <option value="food">Food</option>
                <option value="pleasure">Pleasure</option>
                <option value="education">Education</option>
                <option value="transportation">Transportation</option>
                <option value="gifts">Gifts</option>
            </select>
    </div>
    <ul id="list" class="list">
        <li v-for="transaction in filteredTransactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{ transaction.text }} - {{ transaction.description }} ({{ transaction.category }})
            <span>${{ transaction.amount }}</span>
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
    </ul>
</template>

<!-- <script>
    // options api 
    // export default {
    //     data(){
    //         return {
    //             transactions: [
    //                 { id: 1, text: 'Flower', amount: -19.99 },
    //                 { id: 2, text: 'Salary', amount: 299.97 },
    //                 { id: 3, text: 'Book', amount: -10 },
    //                 { id: 4, text: 'Camera', amount: 150},
    //             ],
    //         }
    //     }
    // }
    
    // compositions api
    export default {
        setup(){
            const transactions = [
                    { id: 1, text: 'Flower', amount: -19.99 },
                    { id: 2, text: 'Salary', amount: 299.97 },
                    { id: 3, text: 'Book', amount: -10 },
                    { id: 4, text: 'Camera', amount: 150},
                ]; 
                return {
                    transactions,
                }
        }
    }
</script> -->
<script setup>
// this define the prop that was pass out in the App.vue

import { defineProps, ref, computed, defineEmits } from 'vue'; 

const emit = defineEmits(['transactionDeleted'])

const props = defineProps({
    transactions: {
        type: Array, 
        required: true
    }
})

const selectedCategory = ref('');

const filteredTransactions = computed(() => {
    if (!selectedCategory.value) {
        return props.transactions;
    }
    return props.transactions.filter(t => t.category === selectedCategory.value);
});


const deleteTransaction = (id) => {
    emit('transactionDeleted', id)
}

</script>

<style scoped>
.filter-control select {
  width: 100%;
  padding: 10px;
  margin: 8px 0 20px; /* Add some margin to the bottom */
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  background-color: white;
}

</style>