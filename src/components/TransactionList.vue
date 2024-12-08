<template>
    <h3>History</h3>
    <ul id="list" class="list">
      <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
        {{ transaction.text }} 
        <span>{{ transaction.amount }}</span>
        <!-- Bind deleteTransaction to the click event and pass the transaction's id -->
        <button class="delete-btn" @click="deleteTransaction(transaction.id) ">x</button>
      </li>     
    </ul>
  </template>
  
  <script setup>
  import { defineProps } from 'vue';
  
  // Declare emits to notify parent component about deletion
  const emit = defineEmits(['transactionDeleted']);
  
  // Define the props that are passed to the component (transactions array)
  const props = defineProps({
    transactions: {
      type: Array,
      required: true,
    },
  });
  
  // Define the deleteTransaction function
  const deleteTransaction = (id) => {
    // Emit the 'transactionDeleted' event with the transaction id
    emit('transactionDeleted', id);
  };
  </script>
  