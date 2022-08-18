<template>
  <div id="pedido-table" v-if="produtos">
    <Message :msg="msg" v-show="msg" />

    <div id="pedido-table-rows">
      <div
        class="pedido-table-row"
        v-for="produto in produtos"
        :key="produto.id"
      >
        <div class="order-number row">
          <strong>id: </strong> {{ produto.id }}
        </div>
        <div class="row"><strong>Código EAN: </strong> {{ produto.ean }}</div>
        <div class="row">
          <strong>Código Interno: </strong> {{ produto.codigointerno }}
        </div>
        <div class="row">
          <strong>Descrição: </strong> {{ produto.descricao }}
        </div>
        <div class="row">
          <strong>Detalhamento: </strong> {{ produto.detalhamento }}
        </div>
        <div class="row">
          <strong>Fabricante: </strong>
          <template v-for="(fabricante, index) in fabricantes" :key="index">
            <template v-if="fabricante.codmarc === produto.fabricante">
              {{ fabricante.tipo }}</template
            >
          </template>
        </div>
        <div class="variacoes">
          <strong>Variações</strong>
          <div
            class="variacoes-item"
            v-for="(variacao, index) in produto.variacoes"
            :key="index"
          >
            <div class="variacoes-cor">
              <template v-for="(cor, index) in cores" :key="index">
                <template v-if="cor.codcor === variacao.cor"
                  ><strong>Cor:</strong> {{ cor.tipo }}</template
                >
              </template>
            </div>
            <div class="variacoes-tamanho">
              <!-- <strong>Tamanho:</strong>{{ variacao.tamanho }} -->
              <div v-for="(tamanho, index) in tamanhos" :key="index">
                <div v-if="tamanho.tipo === variacao.tamanho">
                  <strong>Tamanho:</strong>{{ tamanho.id }}
                </div>
              </div>
            </div>
            <div class="variacoes-preco">
              <strong>Preço:</strong> {{ variacao.preco }}
            </div>
          </div>
        </div>

        <div class="container-btn">
          <router-link
            class="up-btn"
            :to="{ name: 'editar-pedido', params: { id: produto.id } }"
            >Update</router-link
          >

          <!-- <ButtonComp class="delete-btn" @click="deletePedido(produto.id)"
            >Deletar</ButtonComp
          > -->
          <button class="delete-btn" @click="deletePedido(produto.id)">
            Deletar
          </button>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <h2>Não há produtos no momento!</h2>
  </div>
</template>
<script>
import Message from "./Message.vue";
import axios from "axios";

export default {
  name: "Dashboard",
  data() {
    return {
      produtos: null,
      msg: null,
      cores: [],
      fabricantes: [],
      tamanhos: [],
    };
  },
  components: {
    Message,
  },
  methods: {
    getTamanhos() {
      axios.get("http://localhost:3000/tenis").then((response) => {
        this.tamanhos = response.data.tamanhos;
      });
    },

    getFabricantes() {
      axios.get("http://localhost:3000/tenis").then((response) => {
        this.fabricantes = response.data.fabricantes;
      });
    },

    getCores() {
      axios.get("http://localhost:3000/tenis").then((response) => {
        this.cores = response.data.cores;
      });
    },

    getPedidos() {
      axios.get("http://localhost:3000/produtos").then((response) => {
        this.produtos = response.data;
      });
    },

    async deletePedido(id) {
      const req = await fetch(`http://localhost:3000/produtos/${id}`, {
        method: "DELETE",
      });
      const res = await req.json();

      // colocar uma msg de sistema
      this.msg = `Produto removido com sucesso!`;

      // limpar msg
      setTimeout(() => (this.msg = ""), 3000);

      this.getPedidos();
    },
  },
  mounted() {
    this.getPedidos();
    this.getCores();
    this.getFabricantes();
    this.getTamanhos();
  },
};
</script>

<style scoped>
#pedido-table {
  max-width: 1200px;
  margin: 0 auto;
}
#pedido-table-heading,
#pedido-table-rows,
.pedido-table-row {
  flex-wrap: wrap;
}
#pedido-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}
.pedido-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}
#pedido-table-heading div,
.pedido-table-row {
  width: 35%;
}
#pedido-table-heading .order-id,
.pedido-table-row .order-number {
  width: 99.5%;
}

.variacoes {
  border: 1px solid #ccc;
}

.variacoes-item {
  display: flex;
  justify-content: space-between;
  margin: 4px 0px;
}

.variacoes-cor,
.variacoes-tamanho,
.variacoes-preco {
  width: 31%;
}
.row {
  border: 1px solid #ccc;
  margin: 1px;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.container-btn {
  margin-top: 10px;
}
.delete-btn,
.up-btn {
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
.up-btn {
  margin: 10px 10px 0 0;
  text-decoration: none;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
.up-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>