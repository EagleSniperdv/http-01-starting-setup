<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button
          @click="loadExperience"
        >Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading Results...</p>
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">No stored surveys</p>
      <ul >
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  
  data() {
    return {
      results: [ ],
      isLoading: true,
      error: null
    }
  },
  components: {
    SurveyResult,
  },

  methods: {
    loadExperience() {
      this.isLoading = true;
      this.error = null;
      // fetch('https://vue3-app-359e7-default-rtdb.firebaseio.com/surveys.json')
      fetch('https://vue3-app-359e7-default-rtdb.firebaseio.com/surveys.json')
      .then((response) => {
        if (response.ok) {
          return response.json();
        }
      }).then((data) => {
        this.isLoading = false;
        const results = [];

        for (const id in data) {
          results.push({
            id: id,
            name: data[id].name,
            rating: data[id].rating
          });
        }

        this.results = results;
        // console.log(data)
      }).catch((error) => {
        this.isLoading = false;
        console.log(error);
        this.error = 'Loading Error please try again later...'

      });
    }
  },

  mounted() {
    this.loadExperience();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>