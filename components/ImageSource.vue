<template>
    <span class="sub-text">Source: {{getSource()}}</span>
</template>

<script setup lang="ts">
const props = defineProps({
    work: {type: String, required: false},
    url: {type: String, required: false},
    author: {type: String, required: false},
    title: {type: String, required: false}
})

const getSource = () => {
    switch (props.work) {
        case 'spaf':
            return "Stream Processing with Apache Flink by Fabian Hueske"
        default:
            return props.title || props.url
    }
}

</script>

<style scoped>
.sub-text {
    font-size: 0.5rem;
    font-style: italic;
}
</style>