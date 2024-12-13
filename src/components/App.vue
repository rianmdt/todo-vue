<script setup>
import { reactive } from 'vue';
import cabecalho from './cabecalho.vue';
import formulario from './formulario.vue';
import listaDeTarefas from './listaDeTarefas.vue';

    const estado = reactive({
        filtro: 'todas',
        tarefaTemp: '',
        tarefas: [
            {
                titulo: 'Estudar ES6',
                finalizada: false,
            },
            {
                titulo: 'Estudar SASS',
                finalizada: false,
            },
            {
                titulo: 'Ir para a Academia',
                finalizada: true,
            }
        ]
    })

    const getTarefasPendentes = () => {
        return estado.tarefas.filter(tarefa => tarefa.finalizada === false)
    }

    const getTarefasFinalizadas = () => {
        return estado.tarefas.filter(tarefa => tarefa.finalizada)
    }

    const getTarefasFiltradas = () => {
        const { filtro } = estado;

        switch (filtro) {
            case 'pendentes':
                return getTarefasPendentes();
                case 'finalizadas':
                    return getTarefasFinalizadas();
                    default:
                        return estado.tarefas;
        }
    }

    const cadastraTarefa = () => {
        const tarefaNova = {
            titulo: estado.tarefaTemp,
            finalizada: false,
        }
        estado.tarefas.push(tarefaNova);
        estado.tarefaTemp = '';
    }
</script>

<template>
<div class="container">
    <cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <formulario :trocar-filtro="evento => estado.filtro = evento.taregt.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>
    <listaDeTarefas :tarefas="getTarefasFiltradas()" />
</div>


</template>

<style scoped>
    .done {
        text-decoration: line-through;
    }
</style>