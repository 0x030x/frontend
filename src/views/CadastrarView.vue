<template>
  <div class="cadastrar">
    <h1>Cadastrar</h1>
    <button @click="showCadastrarModal = true">Cadastrar Novo CEP</button>
    <ModalDialog v-if="showCadastrarModal" @close="closeCadastrarModal">
      <div class="modal-content">
        <label for="cep">CEP:</label>
        <input v-model="newCep.cep" type="text" id="cep" class="input-field">
        <label for="logradouro">Logradouro:</label>
        <input v-model="newCep.logradouro" type="text" id="logradouro" class="input-field">
        <label for="localidade">Localidade:</label>
        <input v-model="newCep.localidade" type="text" id="localidade" class="input-field">
        <label for="uf">UF:</label>
        <input v-model="newCep.uf" type="text" id="uf" class="input-field">
        <div class="button-container">
          <button @click="saveNewCep">Salvar</button>
          <button @click="closeCadastrarModal">Cancelar</button>
        </div>
      </div>
    </ModalDialog>
  </div>
</template>

<style scoped>
.cadastrar {
  text-align: center;
  margin: 20px;
}

.input-field {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.button-container {
  display: flex;
  justify-content: center;
  margin-top: 15px;
}

button {
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 5px;
}

button:hover {
  background-color: #0056b3;
}

/* Responsividade */
@media (max-width: 768px) {
  .input-field {
    padding: 6px;
  }

  button {
    padding: 3px 6px;
    font-size: 12px;
  }
}
</style>
<script>
import ModalDialog from "@/components/ModalDialog.vue";
import axios from 'axios';

export default {
  name: "CadastrarView",
  components: {
    ModalDialog,
  },
  data() {
    return {
      showCadastrarModal: false,
      newCep: {
        cep: '',
        logradouro: '',
        localidade: '',
        uf: '',
      },
    };
  },
  methods: {
    closeCadastrarModal() {
      this.showCadastrarModal = false;
    },
    saveNewCep() {
      axios.post('http://localhost:8000/api/ceps', this.newCep)
        .then(() => {
          this.$router.push('/listar'); // Redireciona para a página de listagem após cadastrar
        })
        .catch(error => {
          console.error('Erro ao cadastrar novo CEP:', error);
        });
    },
  },
};
</script>
