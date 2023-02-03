<template>
  <div @click="getCountries()">
    Teste
  </div>
</template>

<script>
import axios, {isCancel, AxiosError} from 'axios';

export default {
  data() {
    return {
			countries: null,
    }
  },
  methods: {
    getCountries() {
      var vm = this;
      axios
        .get("http://127.0.0.1:8080/countries")
        .then(response => {
          //var tempReponse = JSON.parse(response.data);
          //vm.countries = tempReponse;
          console.log(response);
        });
    }
  },
  mounted() {
    var vm = this;
    
  }
}

</script>

<style>
  @import '../assets/styles/countries.scss';
</style>