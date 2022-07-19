<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <div>
      <form id="pedido-form" @submit="createPedido">
        <div class="input-container">
          <label for="ean">Código EAN:</label>
          <input
            type="text"
            id="ean"
            name="ean"
            v-model="ean"
            placeholder="Digite o código"
          />
        </div>

        <div class="input-container">
          <label for="nome">Código Interno:</label>
          <input
            type="text"
            id="codigointerno"
            name="codigointerno"
            v-model="codigointerno"
            placeholder="Digite o código interno"
          />
        </div>

        <div class="input-container">
          <label for="nome">Descrição:</label>
          <input
            type="text"
            id="descricao"
            name="descricao"
            v-model="descricao"
            placeholder="Digite a descrição"
          />
        </div>

        <div class="input-container">
          <label for="nome">Detalhamento:</label>
          <input
            type="text"
            id="detalhamento"
            name="detalhamento"
            v-model="detalhamento"
            placeholder="Digite os detalhes"
          />
        </div>

        <div class="input-container">
          <label for="fabricante">Fabricantes:</label>
          <select name="fabricante" id="fabricante" v-model="fabricante">
            <option
              v-for="fabricante in fabricantes"
              :key="fabricante.id"
              :value="fabricante.tipo"
            >
              {{ fabricante.tipo }}
            </option>
          </select>
        </div>

        <div class="input-container">
          <label for="descricao">Cores:</label>
          <select name="descricao" id="descricao" v-model="cor">
            <option v-for="cor in cores" :key="cor.id" :value="cor.tipo">
              {{ cor.tipo }}
            </option>
          </select>
        </div>

        <div class="input-container">
          <label for="descricao">Tamanho:</label>
          <select name="descricao" id="descricao" v-model="tamanho">
            <option
              v-for="tamanho in tamanhos"
              :key="tamanho.id"
              :value="tamanho.tipo"
            >
              {{ tamanho.tipo }}
            </option>
          </select>
        </div>

        <div class="input-container">
          <label for="nome">Preço:</label>
          <input
            type="text"
            id="preco"
            name="preco"
            v-model="preco"
            placeholder="Digite o valor"
          />
        </div>
        <!--
        <div class="input-container">
          <label for="descricao">Detalhamento:</label>
          <select name="descricao" id="descricao" v-model="detalhamento">
            <option
              v-for="detalhamento in detalhamentos"
              :key="detalhamento.id"
              :value="detalhamento.tipo"
            >
              {{ detalhamento.tipo }}
            </option>
          </select>
        </div>
-->
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Cadastrar!" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "PedidoForm",
  // variáveis
  data() {
    return {
      codigointernos: null,
      descricoes: null,
      detalhamentos: null,
      precos: null,
      fabricantes: null,
      cores: null,
      tamanhos: null,
      ean: null,
      descricao: null,
      detalhamento: null,
      preco: null,
      fabricante: null,
      msg: null,
      codigointerno: null,
    };
  },
  // funções
  methods: {
    // Puxa dos dados da API
    async getTenis() {
      const req = await fetch("http://localhost:3000/tenis");
      const data = await req.json();

      this.codigointernos = data.codigointernos;
      this.descricoes = data.descricoes;
      this.detalhamentos = data.detalhamentos;
      this.precos = data.precos;
      this.fabricantes = data.fabricantes;
      this.cores = data.cores;
      this.tamanhos = data.tamanhos;
    },
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

      // limpar msg
      setTimeout(() => (this.msg = ""), 3000);

      // limpar os campos
      this.ean = "";
      this.descricao = "";
      this.detalhamento = "";
      this.preco = "";
      this.fabricante = "";
      this.cor = "";
      this.tamanho = "";
      this.codigointerno = "";
    },
  },
  // executa funções depois de prontas
  mounted() {
    this.getTenis();
  },
  components: {
    Message,
  },
};
</script>

<style scoped>
#pedido-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #fcba03;
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