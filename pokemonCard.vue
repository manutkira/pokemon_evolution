<template>
  <div class="cards">
    <card
      v-for="pokemon in pokemon"
      :key="pokemon.id"
      @click="click(pokemon)"
      :class="{ opace: selected && pokemon.id !== selected }"
    >
      <template v-slot:title> {{ pokemon.name }} #{{ pokemon.id }} </template>
      <template v-slot:content>
        <img :src="pokemon.sprite" />
      </template>
      <template v-slot:description>
        <div v-for="types in pokemon.types" :key="types.id">
          {{ types }}
        </div>
      </template>
      <template v-slot:ability>
        <div v-for="ability in pokemon.ability" :key="ability.id">
          {{ ability }}
        </div>
      </template>
    </card>
  </div>
</template>

<script>
import Card from "./card.vue";

export default {
  components: {
    Card,
  },
  props: {
    selected: {
      type: Number,
    },
    pokemon: {
      type: Array,
    },
  },
  methods: {
    click(pokemon) {
      this.$emit("pokemonClicked", pokemon);
    },
  },
};
</script>

<style scoped>
.cards {
  display: flex;
}

.opace {
  opacity: 0.5;
}

.card:hover {
  opacity: 1;
}

img {
  width: 100%;
}
</style>
