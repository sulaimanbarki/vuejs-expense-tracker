<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li :class="transaction.amount > 0 ? 'plus' : 'minus'" v-for="transaction in transactions"
            :key="transaction.id">
            {{ transaction.text }} <span>$ {{ transaction.amount }}</span><button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
    </ul>
</template>


<script setup>
    import { defineProps } from 'vue';

    const emits = defineEmits('deleteTransactionEmit')

    defineProps({
        transactions: {
            type: Array,
            required: true
        },
    })

    const deleteTransaction = (id) => {
        emits('deleteTransactionEmit', id)
    }
</script>