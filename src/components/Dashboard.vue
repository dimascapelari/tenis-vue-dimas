<template>
  <div id="pedido-table" v-if="pedidos">
    <Message :msg="msg" v-show="msg" />

    <div id="pedido-table-rows">
      <div class="pedido-table-row" v-for="pedido in pedidos" :key="pedido.id">
        <div class="order-number">{{ pedido.id }}</div>
        <div><strong>Código EAN:</strong> {{ pedido.ean }}</div>
        <div><strong>Código Interno:</strong> {{ pedido.codigointerno }}</div>
        <div><strong>Descrição:</strong> {{ pedido.descricao }}</div>
        <div><strong>Detalhamento:</strong> {{ pedido.detalhamento }}</div>
        <div><strong>Fabricante:</strong> {{ pedido.fabricante }}</div>
        <div><strong>Cor:</strong> {{ pedido.cor }}</div>
        <div><strong>Tamanho:</strong> {{ pedido.tamanho }}</div>
        <div><strong>Preço:</strong> {{ pedido.preco }}</div>

        <div>
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
  width: 30%;
}
#pedido-table-heading .order-id,
.pedido-table-row .order-number {
  width: 5%;
}
select {
  padding: 12px 6px;
  margin-right: 12px;
}
.delete-btn {
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

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>