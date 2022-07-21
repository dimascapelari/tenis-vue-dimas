<template>
  <div id="pedido-table" v-if="pedidos">
    <Message :msg="msg" v-show="msg" />

    <div id="pedido-table-rows">
      <div class="pedido-table-row" v-for="pedido in pedidos" :key="pedido.id">
        <div class="order-number row"><strong>id:</strong> {{ pedido.id }}</div>
        <div class="row"><strong>Código EAN:</strong> {{ pedido.ean }}</div>
        <div class="row">
          <strong>Código Interno:</strong> {{ pedido.codigointerno }}
        </div>
        <div class="row">
          <strong>Descrição:</strong> {{ pedido.descricao }}
        </div>
        <div class="row">
          <strong>Detalhamento:</strong> {{ pedido.detalhamento }}
        </div>
        <div class="row">
          <strong>Fabricante:</strong> {{ pedido.fabricante }}
        </div>
        <div class="row"><strong>Cor:</strong> {{ pedido.cor }}</div>
        <div class="row"><strong>Tamanho:</strong> {{ pedido.tamanho }}</div>
        <div class="row"><strong>Preço:</strong> {{ pedido.preco }}</div>

        <div class="container-btn">
          <router-link
            class="up-btn"
            :to="{ name: 'editar-pedido', params: { id: pedido.id } }"
            >Update</router-link
          >

          <button class="delete-btn" @click="deletePedido(pedido.id)">
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

export default {
  name: "Dashboard",
  data() {
    return {
      pedidos: null,
      pedido_id: null,
      status: [],
      msg: null,
    };
  },
  components: {
    Message,
  },
  methods: {
    async getPedidos() {
      const req = await fetch("http://localhost:3000/pedidos");
      const data = await req.json();
      this.pedidos = data;
    },

    async deletePedido(id) {
      const req = await fetch(`http://localhost:3000/pedidos/${id}`, {
        method: "DELETE",
      });
      const res = await req.json();

      // colocar uma msg de sistema
      this.msg = `Pedido removido com sucesso!`;

      // limpar msg
      setTimeout(() => (this.msg = ""), 3000);

      this.getPedidos();
    },
    async updatePedido(event, id) {
      const option = event.target.value;
      const dataJson = JSON.stringify({ status: option });
      const req = await fetch(`http://localhost:3000/pedidos/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });
      const res = await req.json();

      // colocar uma msg de sistema
      this.msg = `Pedido com id${res.id} foi atualizado!`;

      // limpar msg
      setTimeout(() => (this.msg = ""), 3000);

      //console.log(res);
    },
  },
  mounted() {
    this.getPedidos();
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
  /* display: flex; */
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
.pedido-table-row div {
  width: 25%;
}
#pedido-table-heading .order-id,
.pedido-table-row .order-number {
  width: 25%;
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