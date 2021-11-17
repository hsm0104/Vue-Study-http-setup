<template>
  <section>
    <base-card>
      <h2>How was you learning experience?</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">Your Name</label>
          <input type="text" id="name" name="name" v-model.trim="enteredName" />
        </div>
        <h3>My learning experience was ...</h3>
        <div class="form-control">
          <input type="radio" id="rating-poor" value="poor" name="rating" v-model="chosenRating" />
          <label for="rating-poor">Poor</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">Average</label>
        </div>
        <div class="form-control">
          <input type="radio" id="rating-great" value="great" name="rating" v-model="chosenRating" />
          <label for="rating-great">Great</label>
        </div>
        <p
          v-if="invalidInput"
        >One or more input fields are invalid. Please check your provided data.</p>
        <!-- add (technical) error msg -->
        <p v-if="error">
          {{this.error}}
        </p>

        <div>
          <base-button>Submit</base-button>
        </div>
      </form>
    </base-card>
  </section>
</template>

<script>
export default {
  data() {
    return {
      enteredName: '',
      chosenRating: null,
      invalidInput: false,
      // add error 
      error: null,
    };
  },
  // emits: ['survey-submit'],
  methods: {
    submitSurvey() {
      if (this.enteredName === '' || !this.chosenRating) {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;

      // this.$emit('survey-submit', {
      //   userName: this.enteredName,
      //   rating: this.chosenRating,
      // });

          // reset error
        this.error=null
      // fetch firebase api => don't forget (random name).json at the end
      fetch('https://vue-http-demo-1965d-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json',{
        method:'POST',
        headers:{
          'Content-Type': 'application/json'
        },
          // body : JSON.stringify 
        body: JSON.stringify(
           {
            id: new Date().toISOString(), 
            name: this.enteredName,
            rating: this.chosenRating
            }
        )
      })
       // add server error i.e. removing json.stringify
    .then(response=>{
          // add conditions 
      if(response.ok){
        console.log("success saving data");
      }else{
        //  throw mew Error method
        throw new Error("data not saved!!!!!")
      }
    })
        // add technical error. remove .json in url
        .catch(err=> {console.log("error", err)
          this.error = err.message
        })
      this.enteredName = '';
      this.chosenRating = null;
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>