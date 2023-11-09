<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expense="+expense" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import Header from "./components/MyHeader.vue";
import Balance from "./components/AllBalance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";
const transactions = ref([]);
const toast = useToast()
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});
const income = computed(() => {
  return transactions.value
    .filter((transaction) => {
      return transaction.amount > 0;
    })
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => {
      return transaction.amount < 0;
    })
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push(transactionData);
  localStorage.setItem('transactions',JSON.stringify(transactions.value))
  toast.success('Transaction added')
};
const handleTransactionDeleted = (id)=>{
  transactions.value = transactions.value.filter((transaction)=>{
    return transaction.id !== id
  })
  localStorage.setItem('transactions',JSON.stringify(transactions.value))
  toast.success('Transaction deleted')
}
onMounted(()=>{
  if(localStorage.getItem('transactions')){
    transactions.value = JSON.parse(localStorage.getItem('transactions'))
  }
})
</script>

<style></style>
