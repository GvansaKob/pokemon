<template>
  <div class="p-5 text-center">
    <h1 class="text-2xl font-bold">Pokedex</h1>
    <button @click="fetchPokemon" class="bg-blue-500 text-white px-4 py-2 rounded mt-4">
      Fetch Pokémon
    </button>
    <Pokedex :pokemons="pokemons" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import Pokedex from "./components/Pokedex.vue";

const pokemons = ref([]);

const fetchPokemon = async () => {
  const response = await fetch("https://pokeapi.co/api/v2/pokemon?limit=20");
  const data = await response.json();

  const promises = data.results.map(async (pokemon) => {
    const res = await fetch(pokemon.url);
    return res.json();
  });

  pokemons.value = await Promise.all(promises);
};
</script>