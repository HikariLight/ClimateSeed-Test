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

        const units = ["g", "lb", "kg", "metric ton"]

        const unitsData = [["lb", "g", "453.59237"],
                            ["lb", "kg", "0.45359237"],               
                            ["kg", "lb", "2.20462262"],                       
                            ["kg", "metric ton", "0.001"]]

        const convert = (amount : number, from : string, to : string, unitsData : string[][]): number => {
            
            if(from == to) return amount
            
            for(let dataFrame of unitsData){    
                if(from == dataFrame[0] && to == dataFrame[1]) return (amount * Number(dataFrame[2]))
                if(from == dataFrame[1] && to == dataFrame[0]) return (amount ** 2) / (amount * Number(dataFrame[2])) // Using cross multiplication   
            }

            // Using another unit as a catalyst
            const findCatalyst = (unit : string, unitsData : string[][]) => {
                for(let dataFrame of unitsData){
                    if(dataFrame[1] == unit) return dataFrame[0]
                }
                return unit
            }

            let catalyst = findCatalyst(to, unitsData)

            return convert(convert(amount, from, catalyst, unitsData), catalyst, to, unitsData)
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
                result.value = convert(amount.value, from.value, to.value, unitsData)
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