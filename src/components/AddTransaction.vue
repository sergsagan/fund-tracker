<script setup>
import { ref } from 'vue';
import { useToast } from "vue-toastification";

const text = ref('');
const amount = ref('');
const textError = ref(false);
const amountError = ref(false);
const toast = useToast();

const emit = defineEmits(['transactionSubmitted'])

const onSubmit = () => {
  textError.value = !text.value;
  amountError.value = !amount.value;

  if (!text.value || !amount.value) {
    toast.error('Both fields must be filled');
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  emit('transactionSubmitted', transactionData);

  text.value = '';
  amount.value = '';
  textError.value = false;
  amountError.value = false;
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
          v-model="text"
          type="text"
          id="text"
          placeholder="Enter text..."
          :class="{'error': textError}"
      />
    </div>
    <div class="form-control">
      <label for="amount">
        Amount (negative - expense, positive - income)
      </label>
      <input
          v-model="amount"
          type="number"
          id="amount"
          placeholder="Enter amount..."
          :class="{'error': amountError}"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<style scoped>

</style>
