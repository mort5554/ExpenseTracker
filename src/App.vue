<script setup>
import Header from "./components/Header.vue"
import Balance from "./components/Balance.vue"
import IncomeExpenses from "./components/IncomeExpenses.vue"
import TransactionList from "./components/TransactionList.vue"
import AddTransaction from "./components/AddTransaction.vue"

import { useToast } from "vue-toastification"

import {ref, computed, onMounted} from "vue"

const toast = useToast()
//TransactionList.vue
const transactions = ref([])

//Local storage
onMounted(() => {
  const saveTransaction = JSON.parse(localStorage.getItem
  ("transaction"))

  if(saveTransaction){
    transactions.value = saveTransaction
  }
})

//Get totalBalance.vue
//console.log(transactions.value)
    const total = computed(()  => {
      return transactions.value.reduce((acc, transaction) => {
        //console.log(acc, transaction.amount)
        return acc + transaction.amount;
      }, 0)
    })

//Get income IncomeExpenses.vue
const income = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0)
    })

//Get expenses IncomeExpenses.vue
const expenses = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0)
    })

//Add transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })

  saveTransactionToLocalStorage()

  toast.success("TransactionAdded")
}
const  generateUniqueId = () => {
  return Math.floor(Math.random() * 100000)
}

//Delete transaction
const handleTransactionDeleted = (id) => {
  console.log("Delete item id " + id)
  transactions.value = transactions.value.filter(
    (transaction) => 
    transaction.id !== id)

    saveTransactionToLocalStorage()

    toast.success("Transaction deleted")
}

//Save to localstorage
const saveTransactionToLocalStorage = () => {
  localStorage.setItem("transaction", JSON.stringify(transactions.value))
}
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>

<style scoped>

</style>
