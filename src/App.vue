<template>
  <div id="app">
    
    {{elemento1}}
    <busca-informacoes 
        @ao-selecionar='select1' 
        :url="montarUrl"
        :inicial="elemento1_inicial "
        coluna="full_name"/>
    

  </div>
</template>

<script>
import BuscaInformacoes from "./components/BuscaInformacoes.vue";
export default {
  name: 'app',
  components: {
    "busca-informacoes": BuscaInformacoes
  },
  data(){
    return {
      elemento1_inicial : { full_name : "Selecionado", description: "selecionado "},
      elemento1 : { full_name : "Selecionado", description: "selecionado "},
      elemento2 : { full_name : "Selecionado", description: "selecionado "},
      url : "https://api.github.com/search/repositories?q="
    }
  },
  methods: {
    select1(elemento){
      console.log(`select1 - ${JSON.stringify(elemento)}`)
      this.elemento1 = elemento;
    },
    select2(elemento){
      console.log(`select2 - ${JSON.stringify(elemento)}`)
      this.elemento2 = elemento;
    },
    montarUrl(valorBusca){
      return `https://api.github.com/search/repositories?q=${escape(valorBusca)}`;
    }

  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
