<script>
export default {
    data() {
    return {
    };
  },
  methods: {
    closeConfirmation() {
        this.$emit('closeConfirmation')
    },
    deleteUser () {
        this.$emit('deleteUser')
    }
  }
}
</script>

<template>
    <div class="confirmation-fon">
    <div class="wrapper-confirmation">
        <span class="confirmation-text">Вы уверены, что хотите удалить пользователя?</span>
        <div class="confirmation-buttons">
            <button class="confirmation-buttons__button confirmation-buttons__button_true" @click="deleteUser">Да</button>
            <button class="confirmation-buttons__button confirmation-buttons__button_false" @click="closeConfirmation">Нет</button>
        </div>
    </div>
    </div>
</template>

<!-- info.splice(info.indexOf(item), 1) -->

<style lang="scss" scoped>
.confirmation-fon{
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(148, 148, 148, 0.50);
}
body .confirmation-fon{
    overflow-y: hidden;
    height: 100vh;
}
.wrapper-confirmation{
    position: fixed;
    top: 40%;
    left: 40%;
    width: 353px;
    height: 134px;
    background-color: #FFFFFF;
    border-radius: 8px;;
    display: flex;
    flex-direction: column;
    align-items: center;
    box-shadow: 0px 18px 15px rgba(148, 148, 148, 0.15);
}
.confirmation-text{
    font-weight: 400;
    font-size: 12px;
    line-height: 15px;
    color: #000000;
    margin-top: 36px;
}
.confirmation-buttons{
    display: flex;
    gap: 36px;
    margin-top: 24px;
    &__button{
        font-size: 12px;
        line-height: 15px;
        border-radius: 3px;
        padding: 6px 36px;
        border: none;
        &:hover{
            cursor: pointer;
        }
        &_true{
            background: #E0E0E0;
            color: #828282;
        }
        &_false{
            background: #0CB4F1;
            color: #FFFFFF;
        }
    }
}
</style>