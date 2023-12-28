<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="tr in transactions" :key="tr.id" :class="tr.amount > 0 ? 'plus' : 'minus'">
            {{ tr.text }} <span>${{ tr.amount }}</span>
            <button class="delete-btn" @click="deleteHandler(tr.id)">x</button>
        </li>
    </ul>
</template>

<script setup>
import { defineProps } from 'vue';

const emit = defineEmits([
    'removeTransaciton'
])

const props = defineProps({
    transactions: {
        type: Array,
        required: true
    }
})

const deleteHandler = (id) => {
    emit("removeTransaciton", id)
}
</script>