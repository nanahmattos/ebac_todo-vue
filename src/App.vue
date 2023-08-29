<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

    const estado = reactive ({
      filtro: 'todas',
      tarefaTemp: '',
      tarefa: [
        {
        titulo: 'Estudar ES6',
        finalizada: false, 
        },
        {
        titulo: 'Estudar SASS',
        finalizada: false, 
        },
        {
          titulo: 'Ir para a academia',
          finalizada: true, 
        }
      ]
    })
    //essa function é responsavel por pegar as tarefas que nao foram finalizadas
  //a gente usa o filter e pegamos as tarefas.finalizadas que estao como false
    const getTarefasPendentes = () => {
      return estado.tarefa.filter(tarefa => tarefa.finalizada === false)
    }

    const getTarefasFinalizadas = () => {
      return estado.tarefa.filter(tarefa.finalizada)
    }

    const getTarefasFiltradas = () => {
      const {filtro} = estado;

      switch (filtro) {
        case 'pendentes':
          return getTarefasPendentes();
        case 'finalizadas':
          return getTarefasFinalizadas();
        default:
          return estado.tarefa;
      }
    }

    const cadastrarTarefa = () => {
      const tarefaNova = {
        titulo: estado.tarefaTemp,
        finalizada: false,
      }
      estado.tarefa.push(tarefaNova);
      estado.tarefaTemp = ''; //limpa o input 
    }
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefatemp="evento => estado.tarefaTemp = evento.target.value" :cadastrar-tarefa="cadastrarTarefa"/>
    <ListaDeTarefas :tarefa="getTarefasFiltradas()"/>  
  </div>
</template>


