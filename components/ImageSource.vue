<template>
<span class="sub-text">Source: {{props.url}}</span>
</template>
<script setup>
const props = defineProps({
    url: {type: String, required: true}
})
</script>

<style scoped>
.sub-text {
    font-size: 0.5rem;
    font-style: italic;
}
</style>