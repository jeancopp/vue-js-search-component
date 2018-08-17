<template>
    <v-select label="name" 
        :filterable="false" 
        :options="options" 
        @search="onSearch"

        :value="selecionado"
        @input="aoSelecionar"
        
        :resetOnOptionsChange="true"
        >
        <template slot="no-options">
            <slot name="ao-nao-encontrar">
                <div class="d-center">
                    {{ nenhumItemSelecionado.descricao }}
                </div>
            </slot>
        </template>
        <template slot="option" slot-scope="option">
            <slot name="item" :opcao="option">
                <div class="d-center" >
                      {{ option[coluna] }}
                </div>
            </slot>
        </template>
        <template slot="selected-option" scope="option">
            <slot name="item-selecionado">
                <div class="selected d-center">
                    {{ option[coluna] }}
                </div>
            </slot>
        </template>
    </v-select>
</template>

<script>
import axios from "axios";
import lodash from "lodash";
import vSelect from "vue-select";

const nenhumItemSelecionado = Object.freeze({
  descricao: "Nenhum item selecionado"
});

const required = true;

export default {
  name: "BuscaInformacoes",
  props: {
    url: {
      type: [String, Function],
      required
    },
    coluna: {
      type: String,
      required
    },
    inicial: {
      type: Object,
    }
  },
  data() {
    return {
      options: [],
      selecionado: undefined,
      nenhumItemSelecionado,
    };
  },
  methods: {
    search: lodash.debounce((loading, search, vm) => {
      let requestUrl = "";
      if (typeof vm.url === "string") {
        requestUrl = `${vm.url}${search}`;
      } else {
        requestUrl = vm.url(search);
      }
      axios.get(requestUrl).then(res => {
        vm.options = res.data.items;
        loading(false);
      });
    }, 350),
    onSearch(search, loading) {
      loading(true);
      this.search(loading, search, this);
    },
    aoSelecionar(elemento) {
      this.atualizarValor(elemento,this);
    },
    atualizarValor : lodash.debounce((elemento, vm) => {
      vm.selecionado = elemento;
      vm.$emit("ao-selecionar", elemento);
    },350)
  },
  beforeMount(){
    console.log(`b - ${JSON.stringify(this.selecionado)}`)
    this.selecionado = new Object(this.inicial);
    console.log(`c - ${JSON.stringify(this.selecionado)}`)
  },
  components: {
    vSelect
  }
};
</script>

<style scoped>
img {
  height: auto;
  max-width: 2.5rem;
  margin-right: 1rem;
}

.d-center {
  display: flex;
  align-items: center;
}

.selected img {
  width: auto;
  max-height: 23px;
  margin-right: 0.5rem;
}

.v-select .dropdown li {
  border-bottom: 1px solid rgba(112, 128, 144, 0.1);
}

.v-select .dropdown li:last-child {
  border-bottom: none;
}

.v-select .dropdown li a {
  padding: 10px 20px;
  width: 100%;
  font-size: 1.25em;
  color: #3c3c3c;
}

.v-select .dropdown-menu .active > a {
  color: #fff;
}
</style>
