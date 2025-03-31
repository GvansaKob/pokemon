<template>
  <div class="p-5 text-center">
    <h1 class="text-2xl font-bold">Pokedex</h1>
    <button @click="fetchPokemon" class="bg-blue-500 text-white px-4 py-2 rounded mt-4">
      Fetch Pokémon
    </button>

    <Powers v-if="showPowers" @filter="handleFilter" class="powers-container" />

    <Pokedex :pokemons="filteredPokemons" />
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Pokedex from "./components/Pokedex.vue";
import Powers from "./components/Powers.vue";

const pokemons = ref([]);
const selectedType = ref("");
const showPowers = ref(false); // Etat pour gérer l'affichage du composant Powers

// Fonction pour récupérer les Pokémon depuis l'API
const fetchPokemon = async () => {
  const response = await fetch("https://pokeapi.co/api/v2/pokemon?limit=50");
  const data = await response.json();

  const promises = data.results.map(async (pokemon) => {
    const res = await fetch(pokemon.url);
    return res.json();
  });

  pokemons.value = await Promise.all(promises);
  showPowers.value = true; // Afficher Powers après avoir récupéré les Pokémon
};

const filteredPokemons = computed(() => {
  if (!selectedType.value) return pokemons.value;
  return pokemons.value.filter((pokemon) =>
    pokemon.types.some((type) => type.type.name === selectedType.value)
  );
});

const handleFilter = (type) => {
  selectedType.value = type;
};
</script>

<style scoped>
.powers-container {
  margin-top: 20px;
  background-color: white;
  padding: 10px;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
}
</style>
