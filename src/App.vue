<template>
  <div id="app">
    
    <busca-informacoes 
        @ao-selecionar='select' 
        :url="montarUrl"
        :inicial="elemento"
        coluna="full_name"/>
    {{elemento}}
    
    <busca-informacoes 
        @ao-selecionar='select' 
        :url="url" 
        :inicial="elemento"
        coluna="description"/>
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
      elemento : { full_name : "Selecionado", description: "selecionado "},
      url : "https://api.github.com/search/repositories?q="
    }
  },
  methods: {
    select(elemento){
      this.elemento = elemento;
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
