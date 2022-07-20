<template>
  <div class="container">
    <h1>Editar Pedidos</h1>
    <form id="pedido-form">
      <div class="input-container">
        <p><strong>id:</strong> <input type="text" v-model="pedido.id" /></p>
        <p>
          <strong>Código EAN:</strong>
          <input type="text" v-model="pedido.ean" />
        </p>
        <p>
          <strong>Código Interno:</strong>
          <input type="text" v-model="pedido.codigointerno" />
        </p>
        <p>
          <strong>Descrição:</strong>
          <input type="text" v-model="pedido.descricao" />
        </p>
        <p>
          <strong>Detalhamento:</strong>
          <input type="text" v-model="pedido.detalhamento" />
        </p>
        <p>
          <strong>Fabricante:</strong>
          <input type="text" v-model="pedido.fabricante" />
        </p>
        <p><strong>Cor:</strong> <input type="text" v-model="pedido.cor" /></p>
        <p>
          <strong>Tamanho:</strong>
          <input type="text" v-model="pedido.tamanho" />
        </p>
        <p>
          <strong>Preço:</strong> <input type="text" v-model="pedido.preco" />
        </p>
      </div>
      <div class="input-container">
        <input
          type="button"
          @click="atualizaPedido"
          class="submit-btn"
          value="Atualizar!"
        />
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ViewsEditarPedidos",
  data() {
    return {
      pedido: {
        id: "",
        ean: "",
        codigointerno: "",
        descricao: "",
        detalhamento: "",
        fabricante: "",
        cor: "",
        tamanho: "",
        preco: "",
      },
    };
  },

  mounted() {
    console.log(typeof this.$route.params.id);
    this.getPedidos();
  },
  methods: {
    getPedidos() {
      axios.get("http://localhost:3000/pedidos").then((response) => {
        this.pedido = response.data.find(
          (pedido) => pedido.id === parseInt(this.$route.params.id)
        );
      });
    },

    atualizaPedido() {
      axios
        .put(
          `http://localhost:3000/pedidos/${this.$route.params.id}`,
          this.pedido
        )
        .then((response) => {
          console.log(response);
        });
    },
  },
};
</script>

<style scoped>
.container {
  height: 400px;
}

.input-container {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  align-items: flex-end;
}

.input-container input {
  margin: 3px;
}

.submit-btn {
  background-color: #222;
  color: rgb(178, 134, 97);
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>