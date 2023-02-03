<template>
  <div class="countries__wrapper">
    <div class="countries__table">
			<div class="countries__itens" v-for="country in countries">
				<div class="countries__itens-code">
					Código do país: {{ country.code }}
				</div>
				<div class="countries__itens-name">
					Nome do país: {{ country.name }}
				</div>
				<div class="countries__itens-border">
					Fronteiras do país: {{ country.fronteiras.length }}
				</div>
			</div>
		</div>
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
					//Tive que tratar o JSON recebido da API pois não era possivel utilizar o JSON.parse diretamente
					//Retira o "[" e espaço em branco do começo
					var newStr = response.data.substring(2);
					//Retira o "]" e espaço em branco do final
					var newStr1 = newStr.slice(0, -2);
					//Separa os elementos por "}," e transforma em array
					let countriesArray = newStr1.split("},");
					for (let i = 0; i < countriesArray.length - 1; i++) {
    				countriesArray[i] = JSON.parse(countriesArray[i] + "}");
					}
					countriesArray.pop();
          vm.countries = countriesArray;
					vm.sortCountries();
        });
    },
		sortCountries() {
			var vm = this;
			//Para esse problema utilizei como base o algoritmo two pointers, que utiliza dois ponteiros para percorrer o array em duas posições diferentes
			//O primeiro ponteiro começa no primeiro elemento do array
			for (let i = 0; i < vm.countries.length - 1; i++) {
				//Aqui eu defino o maior valor encontrado, como começamos a percorrer agora então o valor maximo é igual a i
  			let max = i;
				//Aqui é onde o segundo ponteiro é criado, ele recebe o valor do primeiro ponteiro + 1 e tambem percorre o array inteiro
  			for (let j = i + 1; j < vm.countries.length; j++) {
					//Caso o numero de fronteiras do segundo ponteiro seja maior ao ponteiro maximo então o ponteiro maximo recebe o valor do segundo ponteiro
    			if (vm.countries[j].fronteiras.length > vm.countries[max].fronteiras.length) {
      			max = j;
    			}
  			}
			//Caso o valor maximo seja diferente do primeiro ponteiro o algoritmo guarda o valor de countries[i] em uma variavel temporaria, o atual countries[i] vai receber o valor maximo
			// e o countries[max] recebera a variavel temporaria que contem o antigo valor de countries[i], assim realizando a troca de posições do array e organizando.
  		if (max !== i) {
    		let temp = vm.countries[i];
    		vm.countries[i] = vm.countries[max];
    		vm.countries[max] = temp;
  		}
		}
		vm.countries = vm.countries;
		}
  },
  mounted() {
    var vm = this;
    vm.getCountries();
  }
}

</script>

<style>
  @import '../assets/styles/countries.scss';
</style>