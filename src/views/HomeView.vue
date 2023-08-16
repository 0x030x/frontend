<template>
  <div class="home">
    <header class="header">
      <img alt="Vue logo" src="../assets/logo.png">
      <h1>Buscar Ceps</h1>
    </header>
    <main class="main">
      <div class="search-container">
        <input v-model="searchQuery" type="text" placeholder="Digite o CEP desejado" />
        <button @click="searchCepsAndOpenModal">Salvar Busca</button>
      </div>
      <div class="results" v-if="showResults">
        <h2>Resultados da pesquisa:</h2>
        <ul>
          <li v-for="cep in searchResults" :key="cep.cep">
            {{ cep.cep }} - {{ cep.logradouro }}, {{ cep.localidade }} - {{ cep.uf }}
          </li>
        </ul>
      </div>
    </main>
    
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HomeView',
  data() {
    return {
      searchQuery: '',
      showResults: false,
      searchResults: []
    };
  },
  methods: {
    searchCepsAndOpenModal() {
      axios.get(`http://localhost:8000/api/ceps/search/${this.searchQuery}`)
        .then(response => {
          const data = response.data;
          
          if (Array.isArray(data) && data.length > 0) {
            this.searchResults = data;
            this.showResults = true;
            this.$emit('open-listing-modal'); 
          } else {
            this.searchResults = [];
            this.showResults = false;
          }
        })
        .catch(error => {
          console.error('Erro ao buscar CEPs:', error);
        });
    }
  }
};
</script>

<style scoped>
.home {
  text-align: center;
  padding: 20px;
}

.header img {
  max-width: 100px;
}

.header h1 {
  font-size: 24px;
  margin-top: 10px;
}

.search-container {
  margin-top: 20px;
}

input[type="text"] {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.results {
  margin-top: 20px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 5px;
}

.footer {
  margin-top: 20px;
}

.footer p {
  font-size: 14px;
  color: #888;
}
</style>
