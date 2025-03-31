<template>
    <div class="grid grid-cols-2 md:grid-cols-3 gap-4 mt-5">
      <div v-for="pokemon in pokemons" :key="pokemon.id" class="border p-4 rounded shadow-lg">
        <h2 class="text-lg font-bold">{{ pokemon.name }}</h2>
        <img :src="pokemon.sprites.front_default" alt="pokemon" class="mx-auto my-2" />
        <p><strong>Compétences :</strong></p>
        <ul>
          <li v-for="ability in pokemon.abilities" :key="ability.ability.name">
            {{ ability.ability.name }}
          </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script setup>
  defineProps(["pokemons"]);
  </script>