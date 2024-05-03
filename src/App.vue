<template>
  <Header />

  <div class="container">
    <Balance :total="total" />
    <IncomeExpense :income="+income" :expense="+expense" />
    <TransactionList :transactions="transactions" @deleteTransactionEmit="handleDeleteTransaction" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import AddTransaction from './components/AddTransaction.vue';
import Balance from './components/Balance.vue';
import Header from './components/Header.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';

import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref([]);

// get transactions from local storage
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions
  }
});

// get total
const total = computed(() => {
  return transactions.value.reduce((sum, transaction) => {
    return sum + transaction.amount;
  }, 0)
});

// get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((sum, transaction) => {
      return sum + transaction.amount;
    }, 0)
    .toFixed(2)
});

// get expense
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((sum, transaction) => {
      return sum + transaction.amount;
    }, 0)
    .toFixed(2)
});

// ------------ emits ---------------
// handle transaction submitted
const handleTransactionSubmitted = (data) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: data.text,
    amount: data.amount
  });

  saveTransactionsToLocalStorage();

  toast.success('Transaction addedd successfully')
}
// handle delete transaction
const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

  saveTransactionsToLocalStorage();

  toast.success('Transaction deleted')
}

// generate unique id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
}

// save transaction to local storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>