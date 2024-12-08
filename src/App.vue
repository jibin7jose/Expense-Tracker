<template>
    <Header />
    <div class="container">
      <Balance :total="total" />
      <IncomeExpenses :income="income" :expenses="expenses" />
      <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
      <AddTransaction @transactionsubmitted="handleTransactionSubmitted" />
    </div>
  </template>
  
  <script setup>
  // Import necessary components and hooks
  import { ref, computed, onMounted } from 'vue';
  import { useToast } from 'vue-toastification';
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';
  
  const toast = useToast();
  
  // Initialize the transactions array
  const transactions = ref([]);
  
  // Load saved transactions from localStorage when the component is mounted
  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
    if (savedTransactions && Array.isArray(savedTransactions)) {
      transactions.value = savedTransactions;
    }
  });
  
  // Get total, income, and expenses
  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => acc + transaction.amount, 0);
  });
  
  const income = computed(() => {
    return transactions.value
      .filter(transaction => transaction.amount > 0)
      .reduce((acc, transaction) => acc + transaction.amount, 0);
  });
  
  const expenses = computed(() => {
    return transactions.value
      .filter(transaction => transaction.amount < 0)
      .reduce((acc, transaction) => acc + transaction.amount, 0);
  });
  
  // Handle adding a transaction
  const handleTransactionSubmitted = (transactionData) => {
    const newTransaction = {
      id: Date.now(), // Use timestamp to ensure uniqueness
      text: transactionData.text,
      amount: parseFloat(transactionData.amount),
    };
    transactions.value.push(newTransaction);
  
    // Save the updated transactions to localStorage
    saveTransactionsToLocalStorage();
    toast.success('Transaction added');
  };
  
  // Handle deleting a transaction
  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(transaction => transaction.id !== id);
    
    // Save the updated transactions to localStorage
    saveTransactionsToLocalStorage();
    toast.success('Transaction deleted');
  };
  
  // Save the current transactions to localStorage
  const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  };
  </script>
  