  <script setup>
  defineProps({
    character: {
      type: Object,
      required: true,
    },
  });
  </script>
  
  <template>
    <div class="character">
      <img :src="character.image" alt="character image" />
      <h3>{{ character.name }}</h3>
      <p>Status: {{ character.status }}</p>
      <p>Species: {{ character.species }}</p>
    </div>
  </template>


  <style scoped>
  .character {
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #ccc;
    padding: 15px;
    border-radius: 10px;
    margin-bottom: 10px;
  }
  .character img {
    max-width: 100%;
    border-radius: 50%;
  }
  </style>