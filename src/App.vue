<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';
import CharacterCard from './components/Character.vue';

const characters = ref([])
const page = ref(1)
const filters = ref({
  name: '',
  status: '',
})
const hasMore = ref(true)

const fetchCharacters = async () => {
  const params = {
    page: page.value,
    name: filters.value.name,
    status: filters.value.status,
  };

  try {
    const response = await axios.get('https://rickandmortyapi.com/api/character', { params })
    characters.value = response.data.results
    hasMore.value = response.data.info.next != null
  } catch (error) {
    console.error(error)
  }
}

const applyFilters = () => {
  page.value = 1
  fetchCharacters()
}

const nextPage = () => {
  if (hasMore.value) {
    page.value++
    fetchCharacters()
  }
}

const prevPage = () => {
  if (page.value > 1) {
    page.value--
    fetchCharacters()
  }
}

onMounted(fetchCharacters)
</script>

<template>
  <div>
    <h1>Rick and Morty Characters</h1>
    <div >
      <input v-model="filters.name" placeholder="Name" />
      <select v-model="filters.status">
        <option value="">All</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <button @click="applyFilters">Apply</button>
    </div>
    <div>
      <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
    </div>
    <div class="page">
      <button @click="prevPage" :disabled="page === 1">Previous</button>
      <span>Page {{ page }}</span>
      <button @click="nextPage" :disabled="!hasMore">Next</button>
    </div>
  </div>
</template>

<style scoped>
input, select {
  margin-right: 10px;
  padding: 5px;
}
button {
  padding: 5px 10px;
  margin-right: 10px;
}

.page{
  margin: 20px 70px;
}
</style>
