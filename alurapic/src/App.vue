<template>
  <div class="corpo">
    <h1 class="centralizado" v-text="titulo"></h1>

    <input type="search" class="filtro"  @input="filtro= $event.target.value" placeholder="filtre pelo título da foto">
    {{ filtro }}

    <ul class="listar-fotos">
      <li class="listar-fotos-item" v-for="foto of fotosComFiltro">
        <meu-painel :titulo="foto.titulo" >
          <imagem-responsiva :url="foto.url" :titulo="foto.titulo"></imagem-responsiva>
        </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
import Painel from './components/shared/painel.vue';
import ImagemResponsiva from './components/shared/imagem-responsiva/imagemResponsiva';

export default {

  components: {
    'meu-painel': Painel,
    'imagem-responsiva': ImagemResponsiva
  },

  data() {
    return {
      titulo: "Breno teste",
      fotos: [],
      filtro: "",
      visivel: true
    };
  },

  computed: {
    fotosComFiltro(){
      if(this.filtro){
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }
    }
  },

  created() {
    let promisse = this.$http.get("http://localhost:3000/v1/fotos");
    promisse.then((res) => res.json()).then((fotos) => (this.fotos = fotos));
  },
};
</script>

<style>
.corpo {
  font-family: Helvetica, sans-serif;
  width: 96%;
  margin: 0 auto;
}

.centralizado {
  text-align: center;
}

.listar-fotos {
  list-style: none;
}
.listar-fotos,
.listar-fotos-item {
  display: inline-block;
}

.filtro{
  display: block;
  width: 100%;
}
</style>
