<script setup>
import { ref } from 'vue';
import { useToast } from "vue-toastification";

const category = ref('');
const amount = ref('');
const categoryError = ref(false);
const amountError = ref(false);
const toast = useToast();

const emit = defineEmits(['transactionSubmitted'])

const onSubmit = () => {
  categoryError.value = !category.value;
  amountError.value = !amount.value;

  if (!category.value || !amount.value) {
    toast.error('Both fields must be filled');
    return;
  }

  const transactionData = {
    category: category.value,
    amount: parseFloat(amount.value)
  }

  emit('transactionSubmitted', transactionData);

  category.value = '';
  amount.value = '';
  categoryError.value = false;
  amountError.value = false;
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Category</label>
      <input
          v-model="category"
          type="text"
          id="category"
          placeholder="Enter category..."
          :class="{'error': categoryError}"
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
