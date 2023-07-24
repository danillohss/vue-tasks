<template>
  <div>
    <div class="row">
      <div class="col-sm-10">
        <h1 class="font-weight-light">Lista de Tarefas</h1>
      </div>
      <div class="col-sm-2">
        <button
          class="btn btn-primary float-right"
          @click="exibirFormulario = !exibirFormulario"
        >
          <i class="fa fa-plus mr-2"></i>
          <span>Criar</span>
        </button>
      </div>
    </div>

    <ul class="list-group" v-if="tarefas.length > 0">
      <TarefasListaIten
        v-for="tarefa in tarefas"
        :key="tarefa.id"
        :tarefa="tarefa"
      />
    </ul>

    <p v-else>Nenhuma tarefa criada.</p>

    <TarefaSalvar v-if="this.exibirFormulario" @salvar="criarTarefa" />
  </div>
</template>

<script>
import api from "../../service/api";
import TarefaSalvar from "./TarefaSalvar.vue";
import TarefasListaIten from "./TarefasListaIten.vue";

export default {
  components: {
    TarefaSalvar,
    TarefasListaIten,
  },
  data() {
    return {
      tarefas: [],
      exibirFormulario: false,
    };
  },
  methods: {
    async getTarefas() {
      await api.get("/tarefas").then((response) => {
        this.tarefas = response.data;
      });
    },
    //MÉTODO POST
    async criarTarefa(tarefa) {
      await api.post(`/tarefas`, tarefa).then((response) => {
        console.log(tarefa)
        this.tarefas.push(response.data);
        this.exibirFormulario = false;
      });
    },
  },
  created() {
    this.getTarefas();
  },
};
</script>
