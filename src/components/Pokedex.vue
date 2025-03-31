<template>
  <div class="pokedex-container">
    <div v-for="pokemon in pokemons" :key="pokemon.id" class="pokemon-card">
      <h2>{{ pokemon.name }}</h2>
      <img :src="pokemon.sprites.front_default" alt="pokemon" />
      <p class="ability-title">Compétences :</p>
      <ul class="ability-list">
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

<style scoped>
.pokedex-container {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 20px;
  padding: 20px;
  justify-items: center;
}

.pokemon-card {
  background: linear-gradient(135deg, #74afd3, #1e4e7e);
  border: 2px solid #6a195d;
  border-radius: 12px;
  padding: 15px;
  text-align: center;
  width: 200px;
  box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.pokemon-card:hover {
  transform: scale(1.05);
  box-shadow: 6px 6px 15px rgba(0, 0, 0, 0.3);
}

.pokemon-card h2 {
  font-size: 20px;
  font-weight: bold;
  text-transform: capitalize;
  color: white;
}

.pokemon-card img {
  width: 100px;
  height: 100px;
  margin: 10px 0;
}

.ability-title {
  font-weight: bold;
  color: white;
  margin-top: 10px;
}

.ability-list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column; 
  align-items: center; 
  gap: 5px; 
}

.ability-list li {
  font-size: 14px;
  color: white;
  background: rgba(255, 255, 255, 0.2);
  padding: 5px;
  border-radius: 5px;
  width: 80%; 
  text-align: center;
  list-style: none;
}
</style>
