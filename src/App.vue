<template>
  <div class="app">
    <h1>Unit Conversion</h1>
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

</style>