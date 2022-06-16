<template>
    <form class="conversionForm">
        <label>Amount</label>
        <input type="text" v-model.number="amount">

        <label>From:</label>
        <select v-model="from">
            <UnitsOptions :units=units />
        </select>
        
        <label>To:</label>
        <select v-model="to">
            <UnitsOptions :units=units />
        </select>

        <button id="convertButton" @click="handleClick">Convert</button>

        <h3 id="result" v-if="result">Result: {{result}} {{ to }}</h3>
    </form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import UnitsOptions from "./UnitOptions.vue"

export default defineComponent({
    name: "ConversionForm",
    components: {
        UnitsOptions
    },
    setup() {

        const amount = ref<number>(0)
        const from = ref<string>("")
        const to = ref<string>("")
        const result = ref<number>(0)

        const units = ["g", "lb", "kg", "metric_ton"]

        const lbToG = ["lb", "g", "453.59237"]
        const lbToKg = ["lb", "kg", "0.45359237"]
        const kgToLb = ["kg", "lb", "2.20462262"]
        const kgToTon =  ["kg", "metric ton", "0.001"]

        const convert = (amount : number, from : string, to : string): number => {
            if(from == "lb" && to=="g"){
                return Number((amount * Number(lbToG[2])).toFixed(2))
            }

            if(from == "lb" && to=="kg"){
                return Number((amount * Number(lbToKg[2])).toFixed(2))
            }

            if(from == "kg" && to=="lb"){
                return Number((amount * Number(kgToLb[2])).toFixed(2))
            }

            if(from == "kg" && to=="metric_ton"){
                return Number((amount * Number(kgToTon[2])).toFixed(2))
            }

            return amount
        }

        const handleClick = (e: Event) =>{
            e.preventDefault()
            result.value = convert(amount.value, from.value, to.value)
        }

        return {
            amount,
            from,
            to,
            units,
            handleClick,
            result
        }
    },
})
</script>

<style>
    .conversionForm{
        background-color: var(--light);
        padding: 1rem;
        border-radius: 15px;
    }

    .conversionForm input{
        margin: 0.5rem auto;
        padding: 0.5rem;
        display: block;
        width: 90%;
    }

    .conversionForm select{
        margin: 0.5rem 0.5rem 0.5rem 0;
        padding: 0.5rem;
    }

    #convertButton{
        display: block;
        padding: 1rem;
        margin: 0.5rem auto;
        border: none;
        border-radius: 15px;
        background-color: var(--success);
        color: white;
    }

    #result{
        color: var(--success)
    }
</style>