<template>
  <div>
    <h1>Rick and Morty Characters</h1>

    <div>
      <input v-model="filters.name" placeholder="Name">
      <select v-model="filters.status">
        <option value="">Any Status</option>
        <option value="Alive">Alive</option>
        <option value="Dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <button @click="applyFilters">Применить</button>
    </div>
    <div class="character-list">
      <CharacterCard v-for="character in characters" :key="character.id" :character="character"/>
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="page === 1">Previous</button>
      <button @click="nextPage" :disabled="!hasNextPage">Next</button>
    </div>

  </div>
</template>

<script>

import { ref, reactive, onMounted } from 'vue';
import axios from 'axios';
import CharacterCard from './CharacterCard.vue';

export default {
  components: {
    CharacterCard
  },
  setup() {
    const characters = ref([]);
    const page = ref(1);
    const hasNextPage = ref(true);
    const filters = reactive({
      name: '',
      status: ''
    });

    const fetchCharacters = async () => {
      const response = await axios.get(`https://rickandmortyapi.com/api/character`, {
        params: {
          page: page.value,
          name: filters.name,
          status: filters.status
        }
      });
      characters.value = response.data.results;
      hasNextPage.value = !!response.data.info.next;
    };

    const applyFilters = () => {
      page.value = 1;
      fetchCharacters();
    };

    const nextPage = () => {
      if (hasNextPage.value) {
        page.value++;
        fetchCharacters();
      }
    };

    const prevPage = () => {
      if (page.value > 1) {
        page.value--;
        fetchCharacters();
      }
    };

    onMounted(fetchCharacters);

    return {
      characters,
      page,
      hasNextPage,
      filters,
      applyFilters,
      nextPage,
      prevPage
    };
  }
};

</script>

<style>
.character-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.pagination {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}
</style>
