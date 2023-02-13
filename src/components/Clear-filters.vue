<script>

export default {
    data() {
    return {
    };
  },
  props: {
    infoSorted: {
        type: Boolean,
        default: true
    },
    search: {
        type: String,
        default: ''
    }
  },
  methods: {
    clearSearch() {
        this.$emit('clearSearch');
    }
  }
};

</script>

<template>
    <button @click="clearSearch" :disabled="search=='' && infoSorted" class="wrapper-clear-filters">
            <img class="wrapper-clear-filters__img" src="/svg/button-clear-filters.svg" alt="метла">
            <span class="wrapper-clear-filters__span">Очистить фильтр</span>
        </button>
</template>

<style lang="scss" scoped>
.wrapper-clear-filters{
    display: flex;
    align-items: center;
    width: max-content;
    background: transparent;
    border: none;
    &:hover{
        cursor: pointer;
    }
    &:disabled{
        display: none;
    }
    &__img{
        height: 16px;
    }
    &__span{
        font-size: 12px;
        line-height: 16px;
        color: #4F4F4F;
    }
}
</style>