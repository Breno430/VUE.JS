<template>
  <div>
    <h1 class="centralizado" v-text="titulo"></h1>

    <input type="search" class="filtro"  @input="filtro= $event.target.value" placeholder="filtre pelo título da foto">
    {{ filtro }}

    <ul class="listar-fotos">
      <li class="listar-fotos-item" v-for="foto of fotosComFiltro">
        <meu-painel :titulo="foto.titulo" >
          <imagem-responsiva :url="foto.url" :titulo="foto.titulo"></imagem-responsiva>
          <meu-botao type="button" rotulo="REMOVER" @click.native="remove(foto)"></meu-botao>
        </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/imagemResponsiva';
import Botao  from '../shared/botao/Botao';

export default {

  components: {
    'meu-painel': Painel,
    'imagem-responsiva': ImagemResponsiva,
    'meu-botao': Botao
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

  methods: {

    remove(foto) {
      if(confirm('Confirmar operacao?')){
      alert('qual foto remover ' + foto.titulo);
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
