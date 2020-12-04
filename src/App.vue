<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Pokedex Logo"
          class="shrink mr-2"
          contain
          src="./assets/Poké_Bola.png"
          transition="scale-transition"
          width="40"
        />
        <h3>POKEDEX</h3>
      </div>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-main>
      <v-row align="center" justify="space-around">
        <v-col cols="12" sm="6" md="6">
          <v-text-field label="Nome do Pokemon" outlined dense v-model="busca"></v-text-field>
        </v-col>
      </v-row>

      <div v-for="(poke, index) in resultadoBusca" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",

  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
      });
  },
  data() {
    return {
      pokemons: [],
      busca: '',
    };
  },

  components: {
    Pokemon,
  },

  methods: {
    exibirMenu: function () {
      this.right = !this.right;
    },
  },
  computed: {
    resultadoBusca: function() {
      if (!/^[A-z]/.test(this.busca)) {
        return this.pokemons;
      }else {
        return this.pokemons.filter(poke => poke.name == this.busca.toLowerCase());
      }
    }
  }
};
</script>
