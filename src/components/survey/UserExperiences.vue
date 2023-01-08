<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="getSurvey">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">loading ....</p>
      <p v-else-if="!isLoading && error">{{error}}</p>
      <p v-else-if="!isLoading && results && results.length==0"> No Stored Data , Add Some</p>
      <ul v-else-if="!isLoading && results && results.length > 0">
        <survey-result v-for="result in results" :key="result.id" :name="result.name"
          :rating="result.rating"></survey-result>
      </ul>
      
     

      
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  // props: ['results'],
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error:null
    }
  },
  methods: {
    getSurvey() {
      this.isLoading = true
      this.error=null
      fetch('http://localhost/learn/vue-backend/public/api/survey').then(function (respons) {
        if (respons.ok) {
          return respons.json()
        }
        else {
          throw new Error('could not get data')
        }
      }).then((data) => {
        const res = [];
        for (const id in data) {
          res.push({ id: id, name: data[id]['name'], rating: data[id]['rating'] })
          this.results = res
          this.isLoading = false
        }
      }).catch((error)=>{
        this.error = error.message,
        this.isLoading=false
      })
    },

  },
  mounted() {
    this.getSurvey()
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