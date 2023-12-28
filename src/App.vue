

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

import { ref, computed } from 'vue'

import { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([
  { id: 1, text: "Flowers", amount: -19.99 },
  { id: 2, text: "Salary", amount: 399.99 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: -80 },
])

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

  toast.success("New transaction saved")
}

const generateId = () => {
  return Math.floor(Math.random() * 1000000)
}

const handleRemoveTransaciton = (id) => {
  transactions.value = transactions.value.filter(arr => arr.id != id)

  toast.success(`Transaction deleted`)
}

</script>

<style scoped></style>

