<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click='getResult'>Load Submitted Experiences</base-button>
      </div>
      <ul>
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
      results: []
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
      axios.get('https://vue-http-demo-77e9e-default-rtdb.firebaseio.com/surveys.json')
        .then(res => {
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
        .catch(err => err);
    }
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
