<template>
  <h3>History</h3>
  <ul id="list" class="list" v-for="t in transactions" :key="t.id">
    <li :class="t.amount>0 ? 'plus' : 'minus'">
      {{ t.text }} <span>$ {{ t.amount }}</span><button @click="deleteTransaction(t.id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>
<script setup>
import {defineProps, defineEmits} from 'vue'
    defineProps({
        transactions : Array,
    })
    const emit = defineEmits(['transactionDeleted'])
    const deleteTransaction = (id)=>{
        emit('transactionDeleted',id)
    }
</script>
