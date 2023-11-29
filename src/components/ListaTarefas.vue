<script>
import CampoDeInput from './CampoDeInput.vue';
import Tarefa from './Tarefa.vue';
import axios, { Axios } from 'axios';

export default {
    components: {
        CampoDeInput,
        Tarefa,
    },
    data() {
        return {
            tarefas: [],
        };
    },
    methods: {
        atualizarConclusao(index, concluida) {
            this.tarefas[index].concluida = concluida;
        },
        removerTarefa(index) {
            this.tarefas.splice(index, 1);
        },
    
        async adicionarTarefa(novaTarefa) {
            if (novaTarefa.trim() !== '') {
                const newPokemon = await this.searchPokemon(novaTarefa)
                this.tarefas.push({ nome: newPokemon.name, img: newPokemon.sprites.front_default });
            }
        },

        async searchPokemon(pokemonName) {
            return axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`).then((response) => {
                return response.data;
            }).catch((error) => {
                console.error(error);
            });
        },
        
    },
};
</script>

<template>
    <div class="body">
        <h2>Lista de Tarefas</h2>
        <campo-de-input class="input" @tarefa-adicionada="adicionarTarefa"></campo-de-input>
        <div class="tarefas">
            <tarefa v-for="carlos in tarefas" :key="carlos.nome" :nome="carlos.nome" :img="carlos.img"
                @tarefa-concluida="atualizarConclusao(index, $event)" @remover-tarefa="removerTarefa(index)"></tarefa>
        </div>
    </div>
</template>

<style scoped>
    .body {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .input {
        width: 30%;
    }
    .tarefas {
        margin-top: 20px;
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 100%;
    }
</style>