<script>
import HeaderText from '@/components/Header-text.vue';
import Sorting from '@/components/Sorting.vue';
import UsersList from '@/components/Users-list.vue';

import axios from 'axios'

export default {
  data() {
    return {
        localInfo: []
    };
  },
  components: {
    HeaderText,
    Sorting,
    UsersList
  },
  beforeMount() {
    axios
      .get('https://5ebbb8e5f2cfeb001697d05c.mockapi.io/users')
      .then(response => (this.localInfo = response.data));
  },
};

</script>

<template>
<HeaderText text="Список пользователей"/>
<UsersList
:localInfo="localInfo"
/>
</template>

<style lang="scss" scoped>
</style>