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
        @editar="selecionarTarefaEditar"
        @deletar="deletarTarefa"
      />
    </ul>

    <p v-else>Nenhuma tarefa criada.</p>

    <TarefaSalvar
      v-if="this.exibirFormulario"
      @salvar="criarTarefa"
      :tarefa="tarefaSelecionada"
      @editar="editarTarefa"
    />
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
      tarefaSelecionada: null,
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
        this.tarefas.push(response.data);
        this.exibirFormulario = false;
        this.resetar();
      });
    },
    selecionarTarefaEditar(tarefa) {
      this.tarefaSelecionada = tarefa;
      this.exibirFormulario = true;
    },
    //MÉTODO PUT
    async editarTarefa(tarefa) {
      await api.put(`/tarefas/${tarefa.id}`, tarefa).then(() => {
        const indice = this.tarefas.findIndex((t) => t.id === tarefa.id);
        this.tarefas.splice(indice, 1, tarefa);
        this.resetar();
      });
    },
    //MÉTODO DELETE
    async deletarTarefa(tarefa) {
      const confirmar = window.confirm(
        `Deseja realmente deletar a tarefa ${tarefa.titulo} ?`
      );
      if (confirmar) {
        api.delete(`/tarefas/${tarefa.id}`).then(() => {
          const indice = this.tarefas.findIndex((t) => t.id === tarefa.id);
          this.tarefas.splice(indice, 1);
        });
      }
    },
    resetar() {
      this.exibirFormulario = false;
      this.tarefaSelecionada = null;
    },
  },
  created() {
    this.getTarefas();
  },
};
</script>
