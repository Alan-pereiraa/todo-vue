<script setup>
  import {reactive} from "vue";
  const state = reactive({
    taskTemporary: "",
    filter: "todas",
    tasks: [
      {
        titulo: "Ir para academia",
        finished: true
      },
      {
        titulo: "Estudar Vue",
        finished: true
      },
      {
        titulo: "Estudar React",
        finished: false
      }
    ]
  });

  const getTasksPending = () => {
    return state.tasks.filter(task => !task.finished)
  }

  const getTasksFinished = () => {
    return state.tasks.filter(task => task.finished)
  }

  const getTaskFiltered = () => {
    const { filter } = state;

    switch(filter){
      case "pendentes":
        return getTasksPending();
      case "finalizadas":
        return getTasksFinished();
      default:
        return state.tasks;
    }
  }

  function registerNewTask(){
    const newTask = {
      titulo: state.taskTemporary,
      finished: false
    }

    state.tasks.push(newTask);
    state.taskTemporary = "";
  }

</script>

<template>
  <div class="container">
    <header class="p-5 my-4 bg-secondary text-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>Você possui {{ getTasksPending().length }} tarefas pendentes</p>
    </header>
    <main>
      <form @submit.prevent="registerNewTask">
        <div class="row">
          <div class="col">
            <input type="text" placeholder="Digite sua nova tarefa..." class="form-control" :value="state.taskTemporary" @change="event => state.taskTemporary = event.target.value">
          </div>
          <div class="col-md-2">
            <button type="submit" class="btn btn-primary">Adicionar</button>
          </div>
          <div class="col-md-3">
            <select class="form-control"  @change="event => state.filter = event.target.value">
              <option value="todas">Todas</option>
              <option value="pendentes">Pendentes</option>
              <option value="finalizadas">Finalizadas</option>
            </select>
          </div>
        </div>
      </form>
      <ul class="list-group mt-4">
        <li class="list-group-item" v-for="task in getTaskFiltered()">
          <input required :checked="task.finished" :id="task.titulo" type="checkbox" @change="event => task.finished = event.target.checked">
          <label :class="{ done: task.finished }" :for="task.titulo" class="ms-3">{{ task.titulo }}</label>
        </li>
      </ul>
    </main>
  </div>
</template>

<style scoped>
  .done{
    text-decoration: line-through;
  }
</style>
