<script setup>
import { defineProps } from 'vue';

const prop = defineProps({
  total:{
    type:Number,
    requred: true,
  }
})
</script>

<template>
    <h4>Your Balance</h4>
      <h1 id="balance">{{ total }}</h1>
</template>