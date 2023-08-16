<template>
    <div class="Listar">
        <h1>Listar</h1>
        <button @click="showModal = true">Abrir Modal</button>
        <ModalDialog v-if="showModal" @close="showModal = false">
            <div v-for="cep in ceps" :key="cep.id" class="cep-card">
                <div class="card-header">{{ cep.cep }}</div>
                <div class="card-body">{{ cep.logradouro }}</div>
                <button @click="editCep(cep)">Editar</button>
                <button @click="deleteCep(cep.id)">Deletar</button>
            </div>
        </ModalDialog>
        <ModalDialog v-if="editingCep" @close="closeEditCepModal">
            <div>
                <label for="cep">CEP:</label>
                <input v-model="editedCep.cep" type="text" id="cep">
                <label for="logradouro">Logradouro:</label>
                <input v-model="editedCep.logradouro" type="text" id="logradouro">
                <label for="localidade">Localidade:</label>
                <input v-model="editedCep.localidade" type="text" id="localidade">
                <label for="uf">UF:</label>
                <input v-model="editedCep.uf" type="text" id="uf">
                <button @click="saveEditedCep">Salvar</button>
                <button @click="cancelEditCep">Cancelar</button>
            </div>
        </ModalDialog>
    </div>
</template>

<script>
import ModalDialog from "@/components/ModalDialog.vue";
import axios from 'axios';

export default {
    name: "ListarView",
    components: {
        ModalDialog,
    },
    data() {
        return {
            showModal: false,
            ceps: [],
            editingCep: false,
            editedCep: {
                id: null,
                cep: '',
                logradouro: '',
                localidade: '',
                uf: '',
            },
        };
    },
    methods: {
        async fetchCeps() {
            try {
                const response = await axios.get('http://localhost:8000/api/ceps');
                this.ceps = response.data;
            } catch (error) {
                console.error('Erro ao buscar CEPs:', error);
            }
        },
        editCep(cep) {
            this.editingCep = true;
            this.editedCep = { ...cep };
        },
        closeEditCepModal() {
            this.editingCep = false;
        },
        saveEditedCep() {
            axios.put(`http://localhost:8000/api/ceps/${this.editedCep.id}`, this.editedCep)
                .then(() => {
                    this.fetchCeps();
                    this.closeEditCepModal();
                })
                .catch(error => {
                    console.error('Erro ao salvar CEP editado:', error);
                });
        },
        cancelEditCep() {
            this.closeEditCepModal();
        },
        deleteCep(id) {
            if (confirm('Tem certeza de que deseja deletar este CEP?')) {
                axios.delete(`http://localhost:8000/api/ceps/${id}`)
                    .then(() => {
                        this.fetchCeps();
                    })
                    .catch(error => {
                        console.error('Erro ao deletar CEP:', error);
                    });
            }
        },
    },
    mounted() {
        this.fetchCeps();
    }
};
</script>

<style scoped>
.Listar {
    text-align: center;
    margin: 20px;
}

.cep-card {
    border: 1px solid #ccc;
    margin-bottom: 10px;
    border-radius: 5px;
    overflow: hidden;
}

.card-header {
    font-weight: bold;
    font-size: 18px;
    padding: 10px;
    background-color: #f5f5f5;
}

.card-body {
    padding: 10px;
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

@media (max-width: 768px) {
    .cep-card {
        padding: 5px;
        font-size: 14px;
    }

    .card-header {
        font-size: 16px;
    }

    .card-body {
        margin-top: 5px;
    }

    button {
        padding: 3px 6px;
        font-size: 12px;
    }
}
</style>
