<template>

    <h4>your Balance</h4>
    <h1 id="balance"><span class="taka-symbol">৳</span> {{ total }} </h1>

</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    total: {
        type: Number,
        required: true,
    },
});


</script>
