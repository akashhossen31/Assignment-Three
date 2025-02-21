<template>
  <div class="container my-5">
    <div class="card shadow-lg p-4 mx-auto" style="max-width: 600px;">
      <h2 class="text-center text-primary">Expense Tracker</h2>
      <AddTransaction @add-transaction="addTransaction" />
      <TransactionList 
        :filtered-transactions="filteredTransactions"
        :delete-transaction="deleteTransaction"
        v-model:filterType="filterType"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';
import 'bootstrap/dist/css/bootstrap.min.css';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

const transactions = ref([]);
const filterType = ref('all');

onMounted(() => {
  const savedTransactions = localStorage.getItem('transactions');
  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});
const saveTransactions = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

const addTransaction = (transaction) => {
  if (transaction.amount > 0) {
    transactions.value.push(transaction);
    saveTransactions();
  } else {
    alert('Amount must be greater than zero.');
  }
};

const deleteTransaction = (index) => {
  transactions.value.splice(index, 1);
  saveTransactions();
};

const filteredTransactions = computed(() => {
  if (filterType.value === 'all') return transactions.value;
  return transactions.value.filter(t => t.type.toLowerCase() === filterType.value);
});
</script>
