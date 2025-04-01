<template>
  <div class="p-5 text-center">
    <h1 class="text-2xl font-bold">Pokedex</h1>

    <button @click="fetchPokemon" class="bg-blue-500 text-white px-4 py-2 rounded mt-4">
      Fetch Pokémon
    </button>

    <div v-if="pokemons.length > 0">
      <input type="text" v-model="searchQuery" placeholder="Rechercher un Pokémon..." class="search-input" />

      <Powers @filter="handleFilter" class="powers-container" />

      <Pokedex :pokemons="filteredPokemons" />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Pokedex from "./components/Pokedex.vue";
import Powers from "./components/Powers.vue";

const pokemons = ref([]);
const selectedType = ref("");
const searchQuery = ref("");

const fetchPokemon = async () => {
  const response = await fetch("https://pokeapi.co/api/v2/pokemon?limit=50");
  const data = await response.json();

  const promises = data.results.map(async (pokemon) => {
    const res = await fetch(pokemon.url);
    return res.json();
  });

  pokemons.value = await Promise.all(promises);
};

const normalizeText = (text) => {
  return text
    .normalize("NFD")
    .replace(/[\u0300-\u036f]/g, "")
    .toLowerCase();
};

const filteredPokemons = computed(() => {
  return pokemons.value.filter((pokemon) => {
    const matchesType =
      !selectedType.value ||
      pokemon.types.some((type) => type.type.name === selectedType.value);

    const matchesSearch =
      !searchQuery.value ||
      normalizeText(pokemon.name).includes(normalizeText(searchQuery.value));

    return matchesType && matchesSearch;
  });
});

const handleFilter = (type) => {
  selectedType.value = type;
};
</script>

<style scoped>
.search-input {
  margin-top: 10px;
  padding: 8px;
  width: 50%;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.powers-container {
  margin-top: 20px;
  background-color: white;
  padding: 10px;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
}
</style>
