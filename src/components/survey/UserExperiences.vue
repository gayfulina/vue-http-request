<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>

      <div>
        <base-button @click='getResult'>Load Submitted Experiences</base-button>
      </div>

      <div v-if='isLoading'>Loading...</div>
      <p v-else-if='!isLoading && error'>{{error}}</p>
      <p v-else-if='!isLoading && (!results || results.length === 0)'>No stored experiences found. Start adding some survey results first</p>
      <ul v-else>
        <survey-result
          v-for='result in results'
          :key='result.id'
          :name='result.name'
          :rating='result.rating'
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import axios from 'axios';
import SurveyResult from './SurveyResult.vue';

export default {
  components: {
    SurveyResult
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
    };
  },
  methods: {
    //   fetchResult() {
    //     fetch('https://vue-http-demo-77e9e-default-rtdb.firebaseio.com/surveys.json')
    //       .then((response) => {
    //         if (response.ok) {
    //           return response.json();
    //         }
    //       })
    //       .then((data) => {
    //         console.log(data);
    //         const tempResults = [];
    //         for (const id in data) {
    //           tempResults.push({
    //             id: id,
    //             name: data[id].name,
    //             rating: data[id].rating
    //           });
    //         }
    //         this.results = tempResults;
    //       });
    //   }
    // },

    getResult() {
      this.isLoading = true;
      this.error = null;
      axios.get('https://vue-http-demo-77e9e-default-rtdb.firebaseio.com/surveys.json')
        .then(res => {
          this.isLoading = false;
          console.log(res.data);
          const tempResults = [];
          for (const id in res.data) {
            tempResults.push({
              id: id,
              name: res.data[id].name,
              rating: res.data[id].rating
            });
          }
          this.results = tempResults;
        })
        .catch(err => {
          console.log(err);
          this.isLoading = false;
          this.error = 'Failed to fetch data - please try again later'
        });
    }
  },
  mounted() {
    this.getResult();
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
