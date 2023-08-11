<template>
  <div id="app">
    <div class="jumbotron jumbotron-fluid">
      <div class="container">
        <h1 class="display-4">Requisições HTTP no Vue</h1>
        <p class="lead">
          Usando a biblioteca Axios para fazer chamadas Ajax à uma API REST.
        </p>
      </div>
    </div>

    <div class="container">
      <TarefasLista />
      <!-- <button class="btn btn-primary mt-4 mb-2" @click="download">
        Baixar imagem
      </button>
      <div
        class="progress"
        role="progressbar"
        :style="{ width: progresso + '%' }"
        aria-label="Basic example"
        :aria-valuenow="progresso"
        aria-valuemin="0"
        aria-valuemax="100"
      >
        {{ progresso }} %
        <div class="progress-bar" style="width: 100%"></div>
      </div>
      <div v-if="img">
        <img :src="img" style="max-width: 100%" />
      </div> -->
    </div>
  </div>
</template>

<script>
import TarefasLista from "./components/TarefasLista.vue";
//import axios from "axios";
export default {
  components: {
    TarefasLista,
  },
  data() {
    return {
      progresso: 0,
      img: undefined,
    };
  },
  // methods: {
  //   //Baixando uma imagem com barra de progresso
  //   async download() {
  //     await axios
  //       .get(
  //         "https://images.unsplash.com/photo-1611859266238-4b98091d9d9b?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8Z3RyJTIwcjM1JTIwJTdDfGVufDB8fDB8fHww&w=1000&q=80",
  //         {
  //           responseType: "blob",
  //           onDownloadProgress: (progressEvent) => {
  //             this.progresso = (
  //               (progressEvent.loaded / progressEvent.total) *
  //               100
  //             ).toFixed(0);
  //           },
  //         }
  //       )
  //       .then((response) => {
  //         const reader = new window.FileReader();
  //         reader.readAsDataURL(response.data);
  //         reader.onload = () => {
  //           this.img = reader.result;
  //         };
  //       });
  //   },
  // },
};
</script>