<template>
    <span class="source">
        Source: {{getTitle()}}
    </span>
    
</template>

<script setup lang="ts">
const props = defineProps({
    author: {type: String, required: true},
    title: {type: String, required: false}
})

const getTitle = () => {
    switch (props.author) {
        case 'hueske':
            return "Stream Processing with Apache Flink by Fabian Hueske"
        default:
            return props.title
    }
}

</script>

<style scoped>
.source {
    font-size: 0.5rem;
    font-style: italic;
}
</style>