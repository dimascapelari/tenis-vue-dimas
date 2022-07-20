<template>
  <div class="container">
    <h1>Editar Pedidos</h1>
    <form id="pedido-form" @submit="createPedido">
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
        <input type="submit" class="submit-btn" value="Atualizar!" />
      </div>
    </form>
  </div>
</template>

<script>
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
    async getPedidos() {
      const req = await fetch("http://localhost:3000/pedidos");
      const data = await req.json();
      this.pedido = data.find(
        (pedido) => pedido.id === parseInt(this.$route.params.id)
      );
      // console.log(this.pedido);
      // console.log(data);
    },
    /*
    // Envia dados para a API
    async createPedido(e) {
      e.preventDefault();

      // console.log("Criou o pedido");

      const data = {
        ean: this.ean,
        descricao: this.descricao,
        detalhamento: this.detalhamento,
        preco: this.preco,
        fabricante: this.fabricante,
        cor: this.cor,
        tamanho: this.tamanho,
        codigointerno: this.codigointerno,
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/pedidos", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();
      // colocar uma msg de sistema
      this.msg = `Pedido Nº ${res.id} realizado com sucesso`;
      console.log(this.msg);

      // limpar msg
      setTimeout(() => (this.msg = ""), 3000);
    },*/
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