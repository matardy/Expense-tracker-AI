<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Balance from './components/Balance.vue'
import Header from './components/Header.vue'; 
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';


import {useToast} from 'vue-toastification';

import { ref, computed, onMounted} from 'vue'; // we need to add this to any object we want to be reactive

const toast = useToast();

const transactions = ref([]);

// This line is enough to have a local storage integration

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if(savedTransactions) {
    transactions.value = savedTransactions;
  }
 })

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount; 

  }, 0); // this zero if for the accumulator to start
})

// Get income 
const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount > 0) // this checks if it is an income
  .reduce((acc, transaction) => {
    return acc + transaction.amount; 

  }, 0)
  .toFixed(2); // this zero if for the accumulator to start
})

// Get expenses
const expenses = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount < 0) // this checks if it is an expense
  .reduce((acc, transaction) => {
    return acc + transaction.amount; 

  }, 0)
  .toFixed(2); // this zero if for the accumulator to start
});

// Add transaction

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    description: transactionData.description, // Include description
    category: transactionData.category,       // Include category
    amount: transactionData.amount
  });

  // Save updated transactions to local storage
  saveTransactionsToLocalStorage();
  toast.success('Transaction added');
};

// Generate unique ID 
const generateUniqueId = () => {
  return Math.floor(Math.random() * 10000)
};

// Delete transaction 
const handleTransactionDeleted = (id) =>{
  console.log(id);
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );

  // local storage integration 
  saveTransactionsToLocalStorage();
  toast.success('Transaction deleted')
}

// Save to localstorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}

</script>