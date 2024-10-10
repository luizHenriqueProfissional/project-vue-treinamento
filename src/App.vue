<script setup>
import { reactive } from 'vue';


const estado = reactive({
  filtro: 'todas',
  tarefaTemporaria: '',
  tarefas: [
    {
    titulo: 'Estudar ES6',
    finalizada: false,
    },
    {
      titulo: 'Estudar Sass',
      finalizada: false,
    },
    {
    titulo:'Academia',
    finalizada:true,
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
  const filtro = estado.filtro;
  switch (filtro){
    case 'pendentes':
    return getTarefasPendentes();

    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
}
const cadastraTarefa = () =>{
  const tarefaNova = {
    titulo: estado.tarefaTemporaria,
    finalizada: false,
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemporaria = '';
}

</script>
<template>
  <div class="container">
    <header class=" p-5 mb-4 mt-4 bg-ligth rounded-3">
      <h1>Minhas Tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="cadastraTarefa">
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemporaria" @change="evento => estado.tarefaTemporaria = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button class="btn btn-primary" type="submit">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select @change="evento => estado.filtro = evento.target.value" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendestes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
  </form>
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
      <input @change="evento => tarefa.finalizada = evento.target.checked"  :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
      <label :class="{ done: tarefa.finalizada}" :for="tarefa.titulo" class="ms-3">
        {{ tarefa.titulo }}
      </label>
    </li>
  </ul>
  </div>

</template>

<style scoped>
.done{
  text-decoration: line-through;
}
</style>
