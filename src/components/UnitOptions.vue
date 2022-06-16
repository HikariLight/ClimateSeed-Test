<template>
    <option v-for="unit in units" :key="unit">{{ unit }}</option>
</template>

<script lang="ts">
export default {
    props: ["units"]
}
</script>