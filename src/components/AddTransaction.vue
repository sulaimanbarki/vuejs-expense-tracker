<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" placeholder="Enter text..." v-model="text" />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="number" id="amount" placeholder="Enter amount..." v-model="amount" />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
    import { ref } from 'vue';
    import { useToast } from 'vue-toastification';

    const toast = useToast();
    const text = ref('');
    const amount = ref(0)

    const emits = defineEmits(['transactionSubmitted'])
    
    const onSubmit = () => {
        if (!text.value || !amount.value) {
            toast.error('Both fields are required');
            return;
        }
        
        const transactionData = {
            text: text.value,
            amount: parseFloat(amount.value)
        };

        emits('transactionSubmitted', transactionData);

        text.value = '';
        amount.value = '';
    }
</script>