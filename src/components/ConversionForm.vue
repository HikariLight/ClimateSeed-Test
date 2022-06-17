<template>
    <form class="conversionForm">
        <label>Amount</label>
        <input :class="{invalid: !amountValid}" type="text" v-model.number="amount">

        <label>From:</label>
        <select :class="{invalid: !fromValid}" v-model="from">
            <UnitsOptions :units=units />
        </select>

        <label>To:</label>
        <select :class="{invalid: !toValid}" v-model="to">
            <UnitsOptions :units=units />
        </select>

        <button id="convertButton" @click="handleClick">Convert</button>

        <h3 id="result" v-if="result">Result: {{result}}</h3>
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

        const amountValid = ref<boolean>(true)
        const fromValid = ref<boolean>(true)
        const toValid = ref<boolean>(true)

        const units = ["g", "lb", "kg", "metric_ton"]

        const lbToG = ["lb", "g", "453.59237"]
        const lbToKg = ["lb", "kg", "0.45359237"]
        const kgToLb = ["kg", "lb", "2.20462262"]
        const kgToTon =  ["kg", "metric ton", "0.001"]

        const convert = (amount : number, from : string, to : string): number => {
            
            switch (from) {
                case "g":
                    if (to === "lb") return Number(((amount ** 2) / (amount * Number(lbToG[2]))).toFixed(2)) // Using cross multiplication
                    if (to === "kg") return Number(convert(convert(amount, from, "lb"), "lb", to).toFixed(2)) // Using lb as a catalyst for conversion
                    if (to === "metric_ton") return Number(convert(convert(amount, from, "kg"), "kg", to).toFixed(2))
                    break

                case "lb":
                    if (to === "g") return Number((amount * Number(lbToG[2])).toFixed(2))
                    if (to === "kg") return Number((amount * Number(lbToKg[2])).toFixed(2))
                    if (to === "metric_ton") return Number(convert(convert(amount, from, "kg"), "kg", to).toFixed(2))
                    break
                
                case "kg":
                    if (to === "lb") return Number((amount * Number(kgToLb[2])).toFixed(2))
                    if (to === "metric_ton") return Number((amount * Number(kgToTon[2])).toFixed(2))
                    if (to === "g") return Number(convert(convert(amount, from, "lb"), "lb", to).toFixed(2))
                    break
                
                    
                case "metric_ton":
                    if (to === "g") return Number(convert(convert(amount, from, "kg"), "kg", to).toFixed(2))
                    if (to === "lb") return Number(convert(convert(amount, from, "kg"), "kg", to).toFixed(2))
                    if (to === "kg") return (amount ** 2) / Number((amount * Number(kgToTon[2])).toFixed(2))
            }
        
            return amount
        }


        const verifyAmount = (amount: number) : boolean => {
            amountValid.value = typeof amount === 'number' && amount >= 0
            return amountValid.value
        }

        const verifyUnit = (type: string, unit: string) : boolean => {
            let valid = units.includes(unit);
            (type == "from") ? fromValid.value = valid : toValid.value = valid
            return valid
        }

        const handleClick = (e: Event) =>{
            e.preventDefault()
            if(verifyAmount(amount.value) && verifyUnit("from", from.value) && verifyUnit("to", to.value)){
                result.value = convert(amount.value, from.value, to.value)
            }
        }

        return {
            amount,
            amountValid,
            from,
            fromValid,
            to,
            toValid,
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

    .invalid{
        border: 0.5px solid var(--danger)
    }

    #result{
        color: var(--success)
    }
</style>