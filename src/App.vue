

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
  </div>
  <IncomeExpences :income="income" :outcome="outcome" />
  <TransactionList :transactions="transactions" @removeTransaciton="handleRemoveTransaciton" />
  <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
</template>

<script setup>
import Header from "./components/Header.vue"
import Balance from "./components/Balance.vue"
import IncomeExpences from "./components/IncomeExpences.vue"
import TransactionList from "./components/TransactionList.vue"
import AddTransaction from "./components/AddTransaction.vue"

import { ref, computed, onMounted } from 'vue'

import { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

const total = computed(() => {
  return transactions.value.reduce((acc, cur) => {
    return acc + cur.amount
  }, 0)
})

const income = computed(() => {
  return transactions.value.filter(a => a.amount > 0).reduce((acc, cur) => {
    return acc + cur.amount
  }, 0)
})

const outcome = computed(() => {
  return transactions.value.filter(a => a.amount < 0).reduce((acc, cur) => {
    return acc + cur.amount
  }, 0)
})

const handleTransactionSubmitted = (data) => {
  transactions.value.push({
    id: generateId(),
    text: data.text,
    amount: data.amount,
  })

  saceToLocalStorage()

  toast.success("New transaction saved")
}

const generateId = () => {
  return Math.floor(Math.random() * 1000000)
}

const handleRemoveTransaciton = (id) => {
  const filteredArr = transactions.value.filter(arr => arr.id != id)
  transactions.value = filteredArr

  saceToLocalStorage()

  toast.success(`Transaction deleted`)
}

const saceToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>

<style scoped></style>

