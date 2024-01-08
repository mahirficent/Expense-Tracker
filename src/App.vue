<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';


//All the Transactions
const transactions = ref([]);

  // {id: 1, text: 'Dress', amount: -2800},
  // {id: 2, text: 'Salary: Taseen', amount: 14000},
  // {id: 3, text: 'Perfume', amount: -1000},
  // {id: 4, text: 'Salary: Somaiya', amount: 10000},
  // {id: 5, text: 'Shoes', amount: -2100},
  // {id: 6, text: 'Salary: TechTrioz', amount: 7000},

const toast = useToast();

onMounted (() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if(savedTransactions) {
    transactions.value = savedTransactions;
  };
});

//Get Total
const total = computed(() => {
  return transactions.value.reduce((acc, transactions) => {
    return acc + transactions.amount;
  }, 0);
});

//Get Income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

//Get Expense
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

//Add Transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount 
  });

  saveTransactionToLocalStorage();
  toast.success('Transaction Added Successfully!!')
};

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

//Delete Transaction
const handleTransactionDeleted = (id) => {
  //console.log(id);
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id != id);
  
  saveTransactionToLocalStorage();
  toast.success("Transaction Deleted Successfully!!");
};

//Save to Local Storage
const saveTransactionToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

</script>