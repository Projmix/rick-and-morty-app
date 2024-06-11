<template>
    <div class="character-card">
      <div class="character-card__img-wrapper">
        <img :src="character.image" :alt="character.name">
      </div>
      <div class="character-card__content-wrapper">
        <div class="section">
          <h2>{{ character.name }}</h2>
          <span class="status">
            <span :class="{'status__icon': true, 'status__alive': character.status === 'Alive', 'status__dead': character.status === 'Dead', 'status__unknown': character.status === 'unknown'}"></span>
            {{ character.status }} - {{ character.species }}
          </span>
        </div>
        <div class="section">
          <span class="text-gray">Last known location: </span>
          <span>{{ character.location.name }}</span>
        </div>
        <div class="section">
          <span class="text-gray">First seen in: </span>
          <span>{{ firstEpisodeName }}</span>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  
  export default {
    props: {
      character: Object
    },
    setup(props) {
      const firstEpisodeName = ref('');
  
      const fetchFirstEpisode = async () => {
        if (props.character.episode.length > 0) {
          const firstEpisodeUrl = props.character.episode[0];
          const response = await axios.get(firstEpisodeUrl);
          firstEpisodeName.value = response.data.name;
        }
      };
  
      onMounted(fetchFirstEpisode);
  
      return {
        firstEpisodeName
      };
    }
  };
  </script>
  
  <style scoped>
  .character-card {
    width: 600px;
    height: 220px;
    display: flex;
    overflow: hidden;
    background: rgb(60, 62, 68);
    border-radius: 0.5rem;
    margin: 0.75rem;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
    color: #ecf0f1;
  }
  
  .character-card__img-wrapper {
    flex-shrink: 0;
  }
  
  .character-card__img-wrapper img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  
  .character-card__content-wrapper {
    padding: 1rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  
  .section {
    margin-bottom: 0.5rem;
  }
  
  .text-gray {
    color: #8f8f8f;
  }
  
  .status {
    display: flex;
    align-items: center;
  }
  
  .status__icon {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    display: inline-block;
    margin-right: 0.5rem;
  }
  
  .status__alive {
    background-color: green;
  }
  
  .status__dead {
    background-color: red;
  }
  
  .status__unknown {
    background-color: gray;
  }
  </style>
  