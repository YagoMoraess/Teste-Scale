<template>
  <div class="users__wrapper" v-if="isNextPageEmpty">
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
  <div class="users__wrapper-empty" v-else>
    <div class="users__empty">A lista não possui segunda página.</div>
  </div>
  <div class="users__pagination-wrapper">
    <div class="users__pagination-title">Pages</div>
    <div class="users__pagination">
      <div class="users__pagination-cards" :class="{'users__pagination-card-current' : currentPageTemp == pages}" v-for="pages in totalPages">
        <div @click="goToPage(pages)">{{ pages }}</div>
        <div @click="goToPage(pages)">{{ currentPage }}</div>
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
      currentPageTemp: 1,
      isNextPageEmpty: false,
    }
  },
  methods: {
    getUsers() {
      var vm = this;
      axios
        .get("http://127.0.0.1:8080/users")
        .then(response => {
          var tempReponse = JSON.parse(response.data);
          vm.userList = tempReponse.data;
          vm.totalPages = tempReponse.total_pages;
          console.log(tempReponse);
        });
    },
    goToPage(page) {
      var vm = this;
      vm.currentPage = page;
      vm.currentPageTemp = page;
    }
  },
  computed: {
    currentPage() {
      var vm = this;
      if(vm.currentPage != 2 && vm.userList.length <= 6) {
        vm.isNextPageEmpty = true;
      } else {
        vm.isNextPageEmpty = false;
      }
    }
  },
  mounted() {
    var vm = this;
    vm.getUsers();
  }
}

</script>

<style>
  @import '../assets/styles/users.scss';
</style>