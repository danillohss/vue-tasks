<template>
  <div>
    <div class="row">
      <div class="col-sm-10">
        <h1 class="font-weight-light">Lista de Tarefas</h1>
      </div>
      <div class="col-sm-2">
        <button class="btn btn-primary float-right" @click="formCriarTarefa()">
          <i class="fa fa-plus mr-2"></i>
          <span>Criar</span>
        </button>
      </div>
    </div>

    <ul class="list-group" v-if="tarefasOrdenadas.length > 0">
      <TarefasListaIten
        v-for="tarefa in tarefasOrdenadas"
        :key="tarefa.id"
        :tarefa="tarefa"
        @editar="selecionarTarefaEditar"
        @deletar="deletarTarefa"
        @concluir="editarTarefa"
      />
    </ul>

    <p v-else-if="msgErro">Nenhuma tarefa criada.</p>

    <div class="alert alert-danger" v-if="msgErro">{{ msgErro }}</div>

    <TarefaSalvar
      v-if="this.exibirFormulario"
      @salvar="criarTarefa"
      :tarefa="tarefaSelecionada"
      @editar="editarTarefa"
      @cancelar="exibirFormulario = false"
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
      msgErro: false,
    };
  },
  computed: {
    tarefasOrdenadas() {
      return this.tarefas
        .slice()
        .sort((a, b) =>
          a.concluido === b.concluido ? 0 : a.concluido ? 1 : -1
        );
    },
  },
  methods: {
    async getTarefas() {
      await api
        .get("/tarefas")
        .then((response) => {
          this.tarefas = response.data;
        })
        .catch((error) => {
          if (error.response) {
            this.msgErro = `O Servidor retornou um erro: ${error.message} ${error.response.statusText}`;
          }
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
    formCriarTarefa() {
      if (this.tarefaSelecionada) {
        this.tarefaSelecionada = null;
        return;
      }

      this.exibirFormulario = !this.exibirFormulario;
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
