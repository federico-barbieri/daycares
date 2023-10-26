<template>
    <div>
        <h2>{{ props.name }}</h2>
        <p>{{ props.address }}</p>
        <p>{{ props.district }}</p>
        <p>{{ props.type }}</p>
    </div>

</template>

<script setup>

import { defineProps } from 'vue';

const props = defineProps({
    name: String,
    district: String,
    type: String,
})

</script>

<style>

</style>