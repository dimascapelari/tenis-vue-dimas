<template>
  <div class="container">
    <h1>Editar Produto</h1>
    <section class="form-container">
      <form id="pedido-form">
        <div class="input-container">
          <div class="input">
            <strong>id:</strong>
            <input type="text" v-model="produto.id" />
          </div>
          <div class="input">
            <strong>Código EAN:</strong>
            <input type="text" v-model="produto.ean" />
          </div>
          <div class="input">
            <strong>Código Interno:</strong>
            <input type="text" v-model="produto.codigointerno" />
          </div>
          <div class="input">
            <strong>Descrição:</strong>
            <input type="text" v-model="produto.descricao" />
          </div>
          <div class="input">
            <strong>Detalhamento:</strong>
            <input type="text" v-model="produto.detalhamento" />
          </div>
          <div class="input">
            <strong>Fabricante:</strong>
            <select v-model="produto.fabricante">
              <template v-for="fabricante in fabricantes" :key="fabricante.id">
                <option :value="fabricante.codmarc">
                  {{ fabricante.tipo }}
                </option>
              </template>
            </select>
          </div>
          <div class="variacoes">
            <strong>Variações</strong><br /><br />
            <div
              class="variacoes-item"
              v-for="(variacao, index) in produto.variacoes"
              :key="index"
            >
              <div class="variacoes-cores">
                <strong>Cor:</strong>
                <select v-model="variacao.cor">
                  <option
                    v-for="(cor, index) in cores"
                    :value="cor.codcor"
                    :key="index"
                  >
                    {{ cor.tipo }}
                  </option>
                </select>
              </div>
              <div class="variacoes-tamanho">
                <strong>Tamanho:</strong>
                <select v-model="variacao.tamanho">
                  <option
                    v-for="(tamanho, index) in tamanhos"
                    :value="tamanho.tipo"
                    :key="index"
                  >
                    {{ tamanho.tipo }}
                  </option>
                </select>
              </div>
              <div class="variacoes-preco">
                <strong>Preço:</strong>
                <input type="text" v-model="variacao.preco" />
              </div>
              <div class="variacoes-acoes">
                <button
                  type="button"
                  class="action action-success"
                  @click="adicionarVariacao"
                >
                  +
                </button>
                <button
                  v-if="produto.variacoes.length > 1"
                  type="button"
                  class="action action-danger"
                  @click="removerVariacao(index)"
                >
                  -
                </button>
              </div>
            </div>
          </div>
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
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ViewsEditarPedidos",
  data() {
    return {
      produto: {
        id: "",
        ean: "",
        codigointerno: "",
        descricao: "",
        detalhamento: "",
        fabricante: "",
        variacoes: [],
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
    adicionarVariacao() {
      const variacao = { cor: "", tamanho: "", preco: "" };
      this.produto.variacoes.push(variacao);
    },

    removerVariacao(indexVariacao) {
      this.produto.variacoes = this.produto.variacoes.filter(
        (variacao, index) => index !== indexVariacao
      );
    },

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
      });
    },

    getPedidos() {
      axios.get("http://localhost:3000/produtos").then((response) => {
        this.produto = response.data.find(
          (produto) => produto.id === parseInt(this.$route.params.id)
        );
      });
    },

    atualizaPedido() {
      axios
        .put(
          `http://localhost:3000/produtos/${this.$route.params.id}`,
          this.produto
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
  height: 100%;
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
  width: 400px;
  margin: 20px;
}

.input-container input {
  margin: 3px;
}

.submit-btn {
  background-color: #222;
  color: rgb(178, 134, 97);
  font-weight: bold;
  border: 2px solid #222;
  border-radius: 10px;
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
  margin: 3px;
}

.action {
  border: 1px solid gray;
  padding: 4px;
  margin: 15px 2px 2px 4px;
  border-radius: 50px;
  width: 40px;
  font-size: 16px;
  font-weight: bolder;
}

.action-success {
  background-color: #c8e6c9;
}

.action-danger {
  background-color: #ffcdd2;
}

.variacoes-item {
  display: flex;
  justify-content: space-between;
}

.variacoes-preco,
.variacoes-cores,
.variacoes-tamanho {
  width: 23%;
}
.variacoes {
  margin-top: 15px;
  border: 1px solid;
}

.input {
  display: flex;
  flex-direction: column;
  margin-top: 15px;
}
.variacoes-preco input {
  width: 100%;
}
.variacoes-cores select {
  width: 100%;
}
.variacoes-tamanho select {
  width: 100%;
}
</style>