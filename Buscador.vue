
<template>
  <div class="ramon">
    <input type="text" v-model="carta" />
    <button @click="Buscar">Buscar Carta</button><br />
    <label for="multilangue">incluir multilingua</label>
    <input
      type="checkbox"
      @change="mudar_lingua"
      :value="multilangue"
      v-model="multilangue"
    />
    <div>
      <ul v-for="(carta, name) in cartas" v-bind:key="name">
        <li>{{ carta.name }}</li>
        <img :src="carta.image_uris.small" /><br />
        <button @click="adicionarNoDeck">Adicionar no deck</button>
        <!--adicionar botão que tem a função de adiconar os itens dessa lista na lista deck-->
      </ul>
      <div>
        <ul v-for="(a, index) in deck" v-bind:key="index">
          <li>{{a.name}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Buscador",

  data() {
    return {
      carta: "",
      cartas: [],
      multilangue: false,
      deck: [],
    };
  },

  methods: {
    mudar_lingua() {
      this.multilangue = !this.multilangue;
      console.log(this.multilangue);
    },
    async Buscar() {
      let url = "";
      if (this.multilangue) {
        url =
          `https://api.scryfall.com/cards/search?include_multilingual=${this.multilangue}&q=` +
          this.carta;
      } else {
        url = `https://api.scryfall.com/cards/search?q=` + this.carta;
      }
      let pesquisa = await fetch(url);
      let resposta = await pesquisa.json();
      let cards = resposta.data;
      this.cartas = cards;
      this.carta = "";
    },

   adicionarNoDeck(){
      let deck = this.deck
      deck.push(this.cartas)
      let novoJson = JSON.parse(JSON.stringify(deck))
      console.log("eu sou o novo deck", novoJson)
    }
  },

}
</script>

