<template>
  <pokemon-card
    :pokemon="starter"
    :selected="selected"
    @pokemonClicked="fetchEvolution"
  />

  <pokemon-card :pokemon="evolution" />
</template>

<script>
import Card from "./card.vue";
import PokemonCard from "./pokemonCard.vue";
const api = "https://pokeapi.co/api/v2/pokemon/";
const STARTER_IDS = [
  1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22,
  23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41,
  42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60,
  61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79,
  80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98,
  99, 100,
];
export default {
  components: {
    Card,
    PokemonCard,
  },
  data() {
    return {
      starter: [],
      evolution: [],
      selected: null,
    };
  },
  async created() {
    const starter = await this.fetchData(STARTER_IDS);
    this.starter = starter;
  },
  methods: {
    async fetchEvolution(pokemon) {
      this.selected = pokemon.id;
      this.evolution = await this.fetchData([pokemon.id + 1, pokemon.id + 2]);
    },
    async fetchData(ids) {
      const responses = await Promise.all(
        ids.map((id) => window.fetch(`${api}${id}`))
      );
      const data = await Promise.all(responses.map((res) => res.json()));
      return data.map((datum) => ({
        id: datum.id,
        name: datum.name,
        sprite: datum.sprites.other["official-artwork"].front_default,
        types: datum.types.map((type) => type.type.name),
        ability: datum.abilities.map((ability) => ability.ability.name),
      }));
    },
  },
};
</script>

<style scoped></style>
