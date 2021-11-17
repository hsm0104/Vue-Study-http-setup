<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="clickedSubmitExperiences">Load Submitted Experiences</base-button>
      </div>
      <!-- add condition loading -->
      <p v-if="loading&&!error">It is loading</p>
      <!-- add condition no data or !results -->
      <p v-else-if="!loading && !results&!error || !loading&&results.length ===0&&!error">No data</p>
      <!-- add error message -->
      <p v-else-if="!loading && error">{{this.error}}</p>



      <!-- add condition data available + results -->
      <ul v-else-if="!loading && results && results.length > 0">
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
  // using api getting data 
  // props: ['results'],
  components: {
    SurveyResult,
  },
    // add result as data
  data (){
    return{
      results: [],
      // add loading data
      loading: false,
      // add error
      error: null,
    }
  },
  methods:{
    // create button click function
   clickedSubmitExperiences(){
    //  loading state change
    this.loading = true
    // reset error
    this.error=null

     fetch('https://vue-http-demo-1965d-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json')
    // 1. response parse json data
    .then(res => res.json())
    // 2. get data 
    .then(data => {console.log("data",data)
    // loading state chage
    this.loading=false

        // put data in result array
    // 1. create result array
    const resultArray = []
    // push id using for
      for (const id in data){
      // push id, name, rating
      resultArray.push({
        id: id,
        name: data[id].name,
        rating: data[id].rating
      })   
    }
    // replace results with result array 
    this.results = resultArray
    console.log(resultArray);
    })

    // add catch error
  .catch(err=> {console.log("error:", err)
    // change loading state
    this.error=err
    this.loading=false
  })
    
  }
},

// add mounted

mounted(){
  this.clickedSubmitExperiences()
}

}
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>