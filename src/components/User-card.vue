<script>

export default {
  data() {
    return {
    };
  },
  props: {
    Id: String,
    Username: String,
    Email: String,
    Registration_date: String,
    Rating: Number
  },
  methods: {
    showConfirmation () {
        this.$emit('showConfirmation', this.Id);
    }
  }
};
</script>

<template>
    <tr>
        <td class="users__item users__item_name">{{ Username }}</td>
        <td class="users__item">{{ Email }}</td>
        <td class="users__item">{{ new Intl.DateTimeFormat("ru", {year: "2-digit", month: "numeric", day: "numeric"}).format(new Date(Registration_date)) }}</td>
        <td class="users__item"> {{ Rating }} </td>
        <td class="users__item users__item_btn"><button class="btn-delete" @click="showConfirmation"><img src="/svg/button-delete.svg" alt="Удалить"></button></td>
    </tr>
</template>

<style lang="scss" scoped>
.users{
&__item{
    font-size: 12px;
    line-height: 16px;
    color: #9EAAB4;
    text-align: left;
    padding: 14px 0 20px;
    border-top: 1px solid #F7F7F7;
    &_name{
        font-weight: 700;
        color: #0CB4F1;
    }
    &_btn{
        display: flex;
        align-items: center;
    }
}
}
.btn-delete{
    background: transparent;
    border: none;
    cursor: pointer;
    height: 16px;
    width: 16px;
    &__img{
        height: 16px;
        width: 16px;
    }
}
</style>