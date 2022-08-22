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

        <div class="input-container">
          <ButtonComp
            type="button"
            class="submit-btn"
            @click="adicionarVariacao"
          >
            Adicionar variação
          </ButtonComp>
        </div>

        <section class="container-variacoes">
          <table class="variacoes">
            <div>
              <h2>Variações</h2>
              <DataTable
                :value="products"
                editMode="row"
                dataKey="id"
                v-model:editingRows="editingRows"
                @row-edit-save="onRowEditSave"
                responsiveLayout="scroll"
              >
                <!-- -------------------------------------- -->
                <CompColumn field="cores" header="Cor" style="width: 20%">
                  <template #editor="{ data, field }">
                    <Dropdown
                      v-model="data[field]"
                      :options="cores"
                      optionLabel="tipo"
                      optionValue="id"
                    />
                  </template>
                  <template #body="slotProps">
                    <template v-for="(cor, index) in cores" :key="index">
                      <span v-if="slotProps.data.cores === cor.id">
                        {{ cor.tipo }}
                      </span>
                    </template>
                    <!-- {{ slotProps.data.cores }} -->
                  </template>
                </CompColumn>
                <!-- -------------------------------------- -->
                <CompColumn
                  field="tamanhos"
                  header="Tamanho"
                  style="width: 20%"
                >
                  <template #editor="{ data, field }">
                    <!-- <InputText v-model="data[field]" /> -->
                    <Dropdown
                      v-model="data[field]"
                      @change="chn"
                      :options="tamanhos"
                      optionLabel="tipo"
                      optionValue="id"
                    />
                  </template>
                  <template #body="slotProps">
                    <template v-for="(tamanho, index) in tamanhos" :key="index">
                      <span v-if="slotProps.data.tamanhos === tamanho.id">
                        {{ tamanho.tipo }}
                      </span>
                    </template>
                    <!-- {{ slotProps.data.tamanhos }} -->
                  </template>
                </CompColumn>
                <!-- -------------------------------------- -->
                <CompColumn field="preco" header="Preço" style="width: 20%">
                  <template #editor="{ data, field }">
                    <InputText v-model="data[field]" />
                  </template>
                </CompColumn>
                <CompColumn
                  :rowEditor="true"
                  style="width: 10%; min-width: 8rem"
                  bodyStyle="text-align:center"
                ></CompColumn>
              </DataTable>
            </div>
          </table>
        </section>
        <div class="input-container">
          <ButtonComp type="submit" class="submit-btn" @click="createPedido"
            >Cadastrar</ButtonComp
          >
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";
import Dropdown from "primevue/dropdown";

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
      variacoes: [{ cores: "", tamanhos: "", preco: "" }],
      txt: "DataTableComp",
      editingRows: [],
      products: null,
      variacao: {},
    };
  },
  watch: {
    editingRows() {
      console.log(this.editingRows);
    },
  },
  // funções
  methods: {
    chn($event) {
      console.log($event);
    },
    adicionarVariacao() {
      this.variacao = { cores: "", tamanhos: "", preco: "" };
      this.variacoes.push(this.variacao);
      console.log(this.variacoes);
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
      console.log(data);

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
    // DataTable
    onRowEditSave(event) {
      let { newData, index } = event;

      this.products[index] = newData;
    },
    getProducts() {
      this.products = this.variacoes;
      // console.log(this.products[0]);
    },
  },
  // executa funções depois de prontas
  mounted() {
    this.getTenis();
    this.getProducts();
  },
  components: {
    Message,
    Dropdown,
  },

  created() {
    this.columns = [
      { field: "code", header: "Code" },
      { field: "name", header: "Name" },
      { field: "quantity", header: "Quantity" },
      { field: "price", header: "Price" },
    ];
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

.container-variacoes {
  display: flex;
  justify-content: center;
}

.variacoes {
  display: flex;
  justify-content: center;
  border: 1px solid rgb(178, 134, 97);
}

.variacoes h2 {
  text-align: center;
  color: rgb(178, 134, 97);
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