<template>
  <div class="mt-4">
    <hr />
    <h2 class="font-weight-light">Salvar Tarefa</h2>
    <form @submit.prevent="salvar()">
      <div class="row">
        <div :class="classeColuna">
          <div class="form-group">
            <label>Título</label>
            <input
              type="text"
              class="form-control"
              v-model="tarefaLocal.titulo"
              placeholder="Título da tarefa"
            />
          </div>
        </div>
        <div class="col-sm-2" v-if="tarefa">
          <div class="form-group">
            <label>Tarefa concluída?</label>
            <button
              type="button"
              class="btn btn-sm d-block"
              :class="classeBotao"
              @click="tarefaLocal.concluido = !tarefaLocal.concluido"
            >
              <i class="fa fa-check"></i>
            </button>
          </div>
        </div>
      </div>

      <button type="submit" class="btn btn-primary">Salvar</button>
      <button type="button" class="btn btn-secondary" style="margin: 0 15px;" @click="cancelar">
        Cancelar
      </button>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    tarefa: {
      type: Object,
      default: undefined,
    },
  },
  data() {
    return {
      tarefaLocal: Object.assign(
        {},
        { titulo: "", concluido: false },
        this.tarefa
      ),
    };
  },
  watch: {
    tarefa() {
      this.tarefaLocal = Object.assign({}, this.tarefa);
    },
  },
  computed: {
    classeBotao() {
      return this.tarefa && this.tarefaLocal.concluido
        ? "btn-success"
        : "btn-secondary";
    },
    classeColuna() {
      return this.tarefa ? "col-sm-10" : "col-sm-12";
    },
  },
  methods: {
    salvar() {
      const operacao = !this.tarefa ? "salvar" : "editar";
      this.$emit(operacao, this.tarefaLocal);
      this.tarefaLocal = { titulo: "", concluido: false };
    },
    cancelar() {
      this.$emit("cancelar");
    },
  },
};
</script>

