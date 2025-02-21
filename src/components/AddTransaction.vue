<template>
    <form @submit.prevent="handleSubmit" class="mb-4">
      <div class="mb-3">
        <label class="form-label fw-bold">Title</label>
        <input v-model="title" type="text" class="form-control" required />
      </div>
  
      <div class="mb-3">
        <label class="form-label fw-bold">Amount ($)</label>
        <input v-model.number="amount" type="number" class="form-control" required />
      </div>
  
      <div class="mb-3">
        <label class="form-label fw-bold">Type</label>
        <select v-model="type" class="form-select" required>
          <option value="income">Income</option>
          <option value="expense">Expense</option>
        </select>
      </div>
  
      <button type="submit" class="btn btn-success w-100">Add Transaction</button>
    </form>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    emits: ['add-transaction'],
    setup(_, { emit }) {
      const title = ref('');
      const amount = ref('');
      const type = ref('income');
  
      const handleSubmit = () => {
        if (!title.value || amount.value <= 0) {
          alert('Please enter valid transaction details.');
          return;
        }
  
        const newTransaction = {
          title: title.value,
          amount: parseFloat(amount.value),
          type: type.value
        };
  
        emit('add-transaction', newTransaction);
  
        title.value = '';
        amount.value = '';
        type.value = 'income';
      };
  
      return { title, amount, type, handleSubmit };
    }
  };
  </script>
  