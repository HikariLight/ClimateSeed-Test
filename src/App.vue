<template>
  <div class="app">
    <h1 id="title">Unit Conversion</h1>
    <ConversionForm />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ConversionForm from "./components/ConversionForm.vue"

export default defineComponent({
  name: 'App',
  components: {
    ConversionForm
  }
});
</script>

<style>
  .app{
    background-color: var(--white);
    border-radius: 15px;
    padding: 1.5rem;
    text-align: center;
    width: 40vh;
  }

  #title{
    color: var(--success);
  }
</style>