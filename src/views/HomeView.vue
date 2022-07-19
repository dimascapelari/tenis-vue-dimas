<template>
  <Banner />
  <div class="main-container">
    <h1>Cadastro de Tênis</h1>
    <PedidoForm />
  </div>
</template>

<script>
import Banner from "../components/Banner.vue";
import PedidoForm from "../components/PedidoForm.vue";

export default {
  nome: "Home",
  components: {
    Banner,
    PedidoForm,
  },
};
</script>
