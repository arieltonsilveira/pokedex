<template>
  <div class="card-pokemon">
    <v-card class="mx-auto" max-width="500" outlined>
      <v-list-item three-line>
        <v-list-item-content>
          <div class="overline mb-4">Pokemon: {{ num }}</div>
          <v-list-item-title class="headline mb-1">
            {{ name | upper }}
          </v-list-item-title>
          <v-list-item-subtitle>{{ pokemo.type }}</v-list-item-subtitle>
        </v-list-item-content>
        <v-img :src="currentImg"> </v-img>
      </v-list-item>
      <v-card-actions>
        <v-btn outlined rounded text @click="mudarSprite"> Mudar Sprite </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
import axios from "axios";
export default {
  created: function () {
    axios.get(this.url).then((res) => {
      this.pokemo.type = res.data.types[0].type.name;
      this.pokemo.front = res.data.sprites.front_default;
      this.pokemo.back = res.data.sprites.back_default;
      this.currentImg = this.pokemo.front;
    });
  },
  data() {
    return {
      isFront: true,
      currentImg: "",
      pokemo: {
        type: "",
        front: "",
        back: "",
      },
    };
  },
  props: {
    num: Number,
    name: String,
    url: String,
  },
  filters: {
    upper: function (value) {
      var newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
  methods: {
    mudarSprite: function() {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemo.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemo.front;
      }
    }
  }
};
</script>

<style scoped>
.card-pokemon {
  margin-top: 20px;
}
</style>