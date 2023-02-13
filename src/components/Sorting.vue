<script>

export default {
    data() {
    return {
    };
  },
  methods: {
    sortByRegistration() {
        this.$emit('sortByRegistration');
    },
    sortByRating () {
        this.$emit('sortByRating');
    }
  },
  props: {
    dateSorted: {
        type: Boolean,
        default: false
    },
    ratingSorted: {
        type: Boolean,
        default: false
    }
  }
};

</script>

<template>
    <div class="wrapper-sorting">
        <span class="wrapper-sorting__name-span">Сортировка:</span>
        <button class="wrapper-sorting__btn " v-bind:class="{ 'wrapper-sorting__btn_actived': dateSorted }" @click="sortByRegistration">Дата регистрации</button>
        <button class="wrapper-sorting__btn wrapper-sorting__btn_rating" v-bind:class="{ 'wrapper-sorting__btn_actived': ratingSorted }" @click="sortByRating">Рейтинг</button>
    </div>
</template>

<style lang="scss" scoped>
.wrapper-sorting{
    display: flex;
    align-items: center;
    margin-top: 72px;
    gap: 9px;
    &__name-span{
        font-size: 10px;
        line-height: 14px;
        color: #9EAAB4;
    }
    &__btn{
        font-size: 10px;
        line-height: 14px;
        background: transparent;
        border: none;
        outline: none;
        color: #9EAAB4;
        border-bottom: 1px dashed #9EAAB4;
        &:hover{
            cursor: pointer;
        }
        &_actived{
            color: #333333;
            border-bottom: 1px dashed #333333;
        }
    }
}
</style>