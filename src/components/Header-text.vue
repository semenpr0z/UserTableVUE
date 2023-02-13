<script setup>
const props = defineProps({
        text: {
            type: String,
            default: 'Заголовок'
        }
    })
</script>

<template>
 <h1 class="header-text">{{ text }}</h1>
</template>

<style lang="scss" scoped>


.header-text{
    font-weight: 700;
    font-size: 24px;
    line-height: 28px;
    margin-top: 12px;
}
</style>