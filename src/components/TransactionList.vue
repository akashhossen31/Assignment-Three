<template>
    <div>
      <h4 class="text-center text-secondary">Transactions List</h4>
  
      <div class="mb-3">
        <label class="form-label">Filter:</label>
        <select :value="filterType" @change="$emit('update:filterType', $event.target.value)" class="form-select">
          <option value="all">All</option>
          <option value="income">Income</option>
          <option value="expense">Expense</option>
        </select>
      </div>
  
      <table class="table table-striped table-hover text-center">
        <thead class="table-dark">
          <tr>
            <th>Title</th>
            <th>Amount</th>
            <th>Type</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="filteredTransactions.length === 0">
            <td colspan="4" class="text-center text-muted">No transactions recorded yet.</td>
          </tr>
          <tr v-for="(transaction, index) in filteredTransactions" :key="index">
            <td class="fw-bold">{{ transaction.title }}</td>
            <td 
              :class="{
                'text-success': transaction.type === 'income',
                'text-danger': transaction.type === 'expense',
                'fw-bold': transaction.amount >= 500
              }"
            >
              ${{ transaction.amount }}
            </td>
            <td class="text-uppercase fw-bold">{{ transaction.type }}</td>
            <td>
              <button class="btn btn-danger btn-sm" @click="deleteTransaction(index)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      filteredTransactions: Array,
      filterType: String,
      deleteTransaction: Function
    }
  };
  </script>
  