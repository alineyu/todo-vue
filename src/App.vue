<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue'
  import Formulario from './components/Formulario.vue'
  import ListaTarefas from './components/ListaTarefas.vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'estudar ES6',
      finalizada: false,
    },
    {
      titulo: 'ir para a academia',
      finalizada: false,
    },
    {
      titulo: 'estudar SASS',
      finalizada: true,
    }
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
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

const cadastrarTarefa = () => {
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
  <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" ></Cabecalho>
  
  <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastrar-tarefa="cadastrarTarefa"></Formulario>
  <ListaTarefas :tarefas="getTarefasFiltradas()" /> 
</div>
</template>

