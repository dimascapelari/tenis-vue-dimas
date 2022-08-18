<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <div>
      <form id="pedido-form">
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
              :value="fabricante.codmarc"
            >
              {{ fabricante.tipo }}
            </option>
          </select>
        </div>

        <!-- <div class="btn-externo">
          <button
            type="button"
            class="action action-success"
            @click="adicionarVariacao"
          >
            V
          </button>
        </div> -->

        <div
          class="variacoes"
          v-for="(variacao, index) in variacoes"
          :key="index"
        >
          <div class="variacoes-input-container">
            <label for="cores">Cores:</label>
            <select name="cores" id="cores" v-model="variacao.cor">
              <option v-for="cor in cores" :key="cor.id" :value="cor.codcor">
                {{ cor.tipo }}
              </option>
            </select>
          </div>

          <div class="variacoes-input-container">
            <label for="tamanho">Tamanho:</label>
            <select name="descricao" id="tamanho" v-model="variacao.tamanho">
              <option
                v-for="tamanho in tamanhos"
                :key="tamanho.id"
                :value="tamanho.tipo"
              >
                {{ tamanho.tipo }}
              </option>
            </select>
          </div>

          <div class="variacoes-input-container">
            <label for="nome">Preço:</label>
            <input
              type="text"
              id="preco"
              name="preco"
              v-model="variacao.preco"
              placeholder="Digite valor"
            />
          </div>

          <div class="variacoes-input-container">
            <button
              type="button"
              class="action action-success"
              @click="adicionarVariacao"
            >
              +
            </button>
            <button
              v-if="variacoes.length > 1"
              type="button"
              class="action action-danger"
              @click="removerVariacao(index)"
            >
              -
            </button>
          </div>
        </div>
        <div class="input-container">
          <ButtonComp type="submit" class="submit-btn" @click="createPedido"
            >Cadastrar</ButtonComp
          >
          <!-- <button class="submit-btn" @click="createPedido">Cadastrar</button> -->
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
      cor: null,
      tamanhos: null,
      ean: null,
      descricao: null,
      detalhamento: null,
      preco: null,
      fabricante: null,
      msg: null,
      codigointerno: null,
      variacoes: [{ cor: "", tamanho: "", preco: "" }],
    };
  },
  // funções
  methods: {
    adicionarVariacao() {
      const variacao = { cor: "", tamanho: "", preco: "" };
      this.variacoes.push(variacao);
    },

    removerVariacao(indexVariacao) {
      this.variacoes = this.variacoes.filter(
        (variacao, index) => index !== indexVariacao
      );
    },

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
    async createPedido() {
      const data = {
        ean: this.ean,
        descricao: this.descricao,
        detalhamento: this.detalhamento,
        fabricante: this.fabricante,
        codigointerno: this.codigointerno,
        variacoes: this.variacoes,
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/produtos", {
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

.variacoes-input-container {
  margin: 4px 0 4px 0;
  width: 95px;
}

.variacoes-input-container label {
  border-left: 4px solid transparent;
}

#preco,
#cores,
#tamanho {
  width: 95px;
  margin-left: 4px;
}

.action {
  border: 1px solid gray;
  padding: 6px;
  margin: 15px 2px 0 4px;
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

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid rgb(178, 134, 97);
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

.variacoes {
  display: flex;
  justify-content: space-between;
  border: 1px solid rgb(178, 134, 97);
}

.submit-btn {
  background-color: #222;
  color: rgb(178, 134, 97);
  font-weight: bold;
  border: 2px solid #222;
  border-radius: 10px;
  padding: 10px;
  font-size: 16px;
  margin: 15px auto;
  cursor: pointer;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>