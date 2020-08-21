<template>
  <div class="corpo">
    <h1 class="centralizado" v-text="titulo"></h1>

    <ul class="listar-fotos">
      <li class="listar-fotos-item" v-for="foto of fotos">
        <div class="painel">
          <h2 class="painel-titulo">{{ foto.titulo }}</h2>
          <div class="painel-conteudo">        
        <img :src="foto.url" :alt="foto.titulo" />
        </div>
        </div>
      </li>
    </ul>
  </div>
</template> 

<script>
export default {
  data() {
    return {
      titulo: "Breno teste",
      fotos: [],
    };
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
</style>