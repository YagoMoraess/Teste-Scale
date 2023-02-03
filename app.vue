<template>
  <div class="users__wrapper">
    <div class="users__card-wrapper" v-for="user in userList">
      <img class="users__card-img" :src="user.avatar"/>
      <div class="users__card-info-wrapper">
        <div class="users__card-info-name-wrapper">
          <div class="users__card-info-name-title">Name: </div>
          <div class="users__card-info-name">{{ user.first_name + " " + user.last_name }}</div>
        </div>
        <div class="users__card-info-email-wrapper">
          <div class="users__card-info-email-title">Email: </div>
          <div class="users__card-info-email">{{ user.email }}</div>
        </div>
        <div class="users__card-info-id-wrapper">
          <div class="users__card-info-id-title">UserId: </div>
          <div class="users__card-info-id">{{ user.id }}</div>
        </div>
      </div>
    </div>
  </div>
  <div class="users__pagination-wrapper">
    <div class="users__pagination-title">Pages</div>
    <div class="users__pagination">
      <div class="users__pagination-cards" :class="{'users__pagination-card-current' : currentPage = pages}" v-for="pages in totalPages">
        {{ pages }}
      </div>
    </div>
  </div>
</template>

<script>
import axios, {isCancel, AxiosError} from 'axios';

export default {
  data() {
    return {
      userList: null,
      totalPages: 0,
      currentPage: 1,
    }
  },
  methods: {
    getUsers() {
      var vm = this;
      axios
        .get("https://reqres.in/api/users")
        .then(response => {
          vm.userList = response.data.data;
          vm.totalPages = response.data.total_pages;
          console.log(response.data);
        });
    },
  },
  mounted() {
      var vm = this;
      vm.getUsers();
  }
}

</script>

<style>
  @import './assets/styles/users.scss';
</style>