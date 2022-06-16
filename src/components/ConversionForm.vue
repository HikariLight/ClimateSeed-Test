<template>
    <form>
        <label>Amount</label>
        <input type="text" v-model.number="amount">

        <label>From:</label>
        <select v-model="from">
            <option value="g">Grams</option>
            <option value="lb">Pounds (lb)</option>
            <option value="kg">Kilograms</option>
            <option value="metric_ton">Metric Tons</option>
        </select>
        
        <label>To:</label>
        <select v-model="to">
            <option value="g">Grams</option>
            <option value="lb">Pounds (lb)</option>
            <option value="kg">Kilograms</option>
            <option value="metric_ton">Metric Tons</option>
        </select>

        <button @click="handleClick">Convert</button>

        <h3 id="result" v-if="result">Result: {{result}} {{ to }}</h3>
    </form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
    name: "ConversionForm",
    setup() {

        const amount = ref<number>()
        const from = ref<string>()
        const to = ref<string>()

        const handleClick = (e: Event) =>{
            e.preventDefault()
        }

        return {
            amount,
            from,
            to,
            handleClick
        }
    },
})
</script>

<style>

</style>