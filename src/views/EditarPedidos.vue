<template>
  <div class="container">
    <h1>Editar Pedidos</h1>
    <section class="form-container">
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
            <select v-model="pedido.fabricante">
              <template v-for="fabricante in fabricantes" :key="fabricante.id">
                <option :value="fabricante.codmarc">
                  {{ fabricante.tipo }}
                </option>
              </template>
            </select>
          </p>

          <p>
            <strong>Cor:</strong>
            <select v-model="pedido.cor">
              <template v-for="cor in cores" :key="cor.id">
                <option :value="cor.codcor">
                  {{ cor.tipo }}
                </option>
              </template>
            </select>
          </p>
          <p>
            <strong>Tamanho:</strong>
            <select v-model="pedido.tamanho">
              <template v-for="tamanho in tamanhos" :key="tamanho.id">
                <option :value="tamanho.tipo">
                  {{ tamanho.tipo }}
                </option>
              </template>
            </select>
            <!-- <input type="text" v-model="pedido.tamanho" /> -->
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
    </section>
    <!-- teste -------------------------- -->

    <!-- <ul>
      <li v-for="ped in this.pedido" :key="ped.id">
        <p>{{ ped }}</p>
      </li>
    </ul> -->
    <!-- -------------------------------- -->
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
      fabricantes: [],
      cores: [],
      tamanhos: [],
    };
  },

  mounted() {
    console.log(typeof this.$route.params.id);
    this.getPedidos();
    this.getFabricantes();
    this.getCores();
    this.getTamanhos();
  },

  methods: {
    getTamanhos() {
      axios.get("http://localhost:3000/tenis").then((response) => {
        this.tamanhos = response.data.tamanhos;
      });
    },

    getCores() {
      axios.get("http://localhost:3000/tenis").then((response) => {
        this.cores = response.data.cores;
      });
    },

    getFabricantes() {
      axios.get("http://localhost:3000/tenis").then((response) => {
        this.fabricantes = response.data.fabricantes;
        console.log(this.fabricantes.fabricantes);
      });
    },

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

.container h1 {
  margin-top: 30px;
}

.form-container {
  display: flex;
  justify-content: center;
  gap: 100px;
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

select {
  width: 153px;
  margin: 3px;
}
</style>