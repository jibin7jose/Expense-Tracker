<template>
  <form @submit.prevent="submitTransaction">
    <div class="form-control">
      <h3>Add new transition </h3>
          <label for="text">Text</label>
          <input type="text" id="text" v-model="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
          <label for="amount"
            >Amount <br />
            (negative - expense, positive - income)</label
          >
          <input type="text" id="amount" v-model="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

// Declare the emits option
const emit = defineEmits(['transactionsubmitted']); // Declare that this component emits the 'transactionsubmitted' event

const text = ref('');
const amount = ref('');

// Function to handle form submission
const submitTransaction = () => {
  // Validate text and amount
  if (text.value && !isNaN(amount.value)) {
    emit('transactionsubmitted', { text: text.value, amount: amount.value });
    text.value = '';  // Reset input after submission
    amount.value = '';  // Reset amount after submission
  }
};
</script>
