<template>
  <Header />

  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TranscationList :transactions="transactions" @transactionDeleted ="handleTransactionSubmitted" />
    <AddTransaction 

    @transactionSubmitted="handleTransactionSubmitted"

    />
    
  </div>
</template>


<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TranscationList from './components/TranscationList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed } from 'vue';
const transactions = ref([
    { id: 1, text: 'Flower', amount: -19.99 },
    { id: 2, text: 'Salary', amount: 299.99 },
    { id: 3, text: 'Book', amount: -10 },
    { id: 4, text: 'Camer', amount: 150 },

]);

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount > 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
});

// Get income
const expenses = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount < 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
});


//add transction

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,

  });
  console.log(generateUniqueId());
}

// generate id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 10000000)
};

const handleTransactionDeleted = (id) => {
  console.log(id);
}
</script>