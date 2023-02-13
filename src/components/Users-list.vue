<script>
import UserCard from '@/components/User-card.vue';
import Confirmation from '@/components/Confirmation.vue';
import Sorting from '@/components/Sorting.vue';
import ClearFilters from '@/components/Clear-filters.vue';

import axios from 'axios';

export default {
    data() {
    return {
      info: [],
      localInfoClone: [],
      isConfirmationVisible: false,
      idOfDeleting: null,
      pageNumber: 0,
      size: 5,
      search: '',
      counterSortingRegistration: 1,
      counterSortingRating: 1,
      infoSorted: true,
      dateSorted: false,
      ratingSorted: false
    };
  },
  beforeMount() {
    axios
      .get('https://5ebbb8e5f2cfeb001697d05c.mockapi.io/users')
      .then(response => (this.info = response.data));
  },
  components: {
    UserCard,
    Confirmation,
    Sorting,
    ClearFilters
  },
  props: {
    localInfo: {
        type: Array,
        default: []
    }
  },
  watch: {
    search() {
        this.pageNumber = 0
    }
  },
  computed: {
    pageCount(){
        let l = this.info.filter(item => (item.username.toUpperCase().indexOf(this.search.toUpperCase()) !== -1 || item.email.toUpperCase().indexOf(this.search.toUpperCase()) !== -1)).length,
            s = this.size;
        return Math.ceil(l/s);
    },
    paginatedData(){
        const start = this.pageNumber * this.size, end = start + this.size;
        return this.info.filter(item => (item.username.toUpperCase().indexOf(this.search.toUpperCase()) !== -1 || item.email.toUpperCase().indexOf(this.search.toUpperCase()) !== -1)).slice(start, end);
    }
  },
  methods: {
    showConfirmation(Id) {
        this.isConfirmationVisible = true;
        this.idOfDeleting = Id;
        document.body.style.height = '100vh';
        document.body.style.overflowY = 'hidden';
    },
    closeConfirmation() {
        this.isConfirmationVisible = false;
        document.body.style.removeProperty("height");
        document.body.style.removeProperty("overflow-y");
    },
    deleteUser() {
        if (this.localInfoClone.length==0){
            this.localInfoClone = this.localInfo;
            this.localInfoClone = this.localInfoClone.filter(item => item.id != this.idOfDeleting);
        }else{
            this.localInfoClone = this.localInfoClone.filter(item => item.id != this.idOfDeleting);
        }
        this.info = this.info.filter(item => item.id != this.idOfDeleting);
        this.isConfirmationVisible = false;
        document.body.style.removeProperty("height");
        document.body.style.removeProperty("overflow-y");
    },
    nextPage(){
        this.pageNumber++;
    },
    prevPage(){
        this.pageNumber--;
    },
    resetPageNumber() {
        if(this.search !== '') {
            this.pageNumber = 0;
        }
    },
    sortByRegistration() {
        this.infoSorted = false;
        this.dateSorted = true;
        this.ratingSorted = false;
        if (this.localInfoClone.length==0){
            this.localInfoClone = this.localInfo;
        }
            if(this.counterSortingRegistration%2 == 1){
                this.counterSortingRegistration++;
                this.info = this.info.sort(function(a, b){
                    return new Date(b.registration_date) - new Date(a.registration_date);
                })
            }else{
                this.counterSortingRegistration--;
                this.info = this.info.sort(function(a, b){
                    return new Date(a.registration_date) - new Date(b.registration_date);
                })
                };
    },
    sortByRating() {
        this.infoSorted = false;
        this.ratingSorted = true;
        this.dateSorted = false;
        if (this.localInfoClone.length==0){
            this.localInfoClone = this.localInfo;
        }
            if(this.counterSortingRating%2 == 1){
                this.counterSortingRating++;
                this.info = this.info.sort(function(a, b){
                    return b.rating - a.rating;
                })
                }else{
                    this.counterSortingRating--;
                    this.info = this.info.sort(function(a, b){
                        return a.rating - b.rating;
                    })
                };
    },
    clearSearch() {
        this.search = '';
        this.info = this.localInfoClone;
        this.infoSorted = true;
        this.dateSorted = false;
        this.ratingSorted = false;
    }
  }
};

</script>

<template>
    <Confirmation
    v-if="isConfirmationVisible"
    @closeConfirmation="closeConfirmation"
    @deleteUser="deleteUser"
    />
    <div class="wrapper-search-form">
        <div class="wrapper-btn-input">
            <img class="wrapper-btn-input__btn" src="/svg/button-search.svg" alt="Поиск">
            <input class="wrapper-btn-input__input" v-model="search" type="text" placeholder="Поиск по имени или e-mail">
        </div>
        <ClearFilters
        @clearSearch="clearSearch"
        :infoSorted="infoSorted"
        :search="search"
        />
    </div>
    <Sorting
    @sortByRegistration="sortByRegistration"
    @sortByRating="sortByRating"
    :ratingSorted="ratingSorted"
    :dateSorted="dateSorted"
    />
    <table class="wrapper-users-list">
        <thead class="users-characteristics">
            <tr>
                <th class="users-characteristics__item">Имя пользователя</th>
                <th class="users-characteristics__item">E-mail</th>
                <th class="users-characteristics__item">Дата регистрации</th>
                <th class="users-characteristics__item">Рейтинг</th>
            </tr>
        </thead>
        <tbody class="users">
            <TransitionGroup name="list"><UserCard
            v-for="item in paginatedData"
            :key="item.id"
            :Id="item.id"
            :Username="item.username"
            :Email="item.email"
            :Registration_date="item.registration_date"
            :Rating="item.rating"
            @showConfirmation="showConfirmation"
            > </UserCard>
            </TransitionGroup>
        </tbody>
    </table>
    <div class="buttons-pag">
        <button class="buttons-pag__button" :disabled="pageNumber==0" @click="prevPage"> Предыдущая страница</button>
        <button class="buttons-pag__button" :disabled="pageNumber >= pageCount-1" @click="nextPage">Следующая страница</button>
    </div>
</template>

<style lang="scss" scoped>
.wrapper-search-form{
    margin-top: 20px;
    background: #FFFFFF;
    box-shadow: 0px 18px 15px rgba(148, 148, 148, 0.15);
    border-radius: 7px;
    height: 102px;
    display: flex;
    flex-direction: column;
    padding: 0 44px 0 16px;
}
.wrapper-btn-input{
display: flex;
align-items: center;
background: #ECEFF0;
border-radius: 4px;
margin: 12px 0 24px 0;
height: 34px;
&__btn{
    width: 24px;
    padding: 5px 0 5px 3px;
}
&__input{
    border: none;
    background-color: transparent;
    width: 100%;
    padding: 0 10px;
    font-size: 12px;
    line-height: 16px;
    color: #9EAAB4;
    &:focus{
            outline: none;
            background-color: transparent;
        }
}
}

.wrapper-users-list{
    margin-top: 15px;
    background: #FFFFFF;
    border-radius: 7px;
    box-shadow: 0px 18px 15px rgba(148, 148, 148, 0.15);
    padding: 16px;
    width: 100%;
}

.users-characteristics{
    &__item{
        font-size: 10px;
        line-height: 14px;
        color: #9EAAB4;
        text-align: left;
        padding-bottom: 22px;
    }
}
.users{
padding-top: 22px;
}

.buttons-pag{
    margin: 24px auto;
    padding: 10px;
    display: flex;
    gap: 36px;
    margin-top: 24px;
    width: max-content;
    background-color: #FFFFFF;
    border-radius: 8px;
    box-shadow: 0px 18px 15px rgba(148, 148, 148, 0.15);
    &__button{
        font-size: 12px;
        line-height: 15px;
        border-radius: 3px;
        padding: 6px 36px;
        border: none;
        background: #0CB4F1;
        color: #FFFFFF;
        transition: 0.4s;
        &:hover{
            cursor: pointer;
        }
        &:active{
            background: #0cb4f1c2;
        }
        &:disabled{
            background: #E0E0E0;
            color: #828282;
            cursor: not-allowed;
        }
    }
}

.list-move{}
.list-enter-active{
  transition: all 0.5s ease;
}

.list-enter-from{
    opacity: 0;
    transform: translateX(30px);
}
.list-leave-to {
  opacity: 0;
}

.list-leave-active {
}

</style>