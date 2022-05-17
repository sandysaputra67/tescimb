<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>EDIT POST</h5>
          <hr>
          <b-form @submit="update">
            <b-form-group label="Title Post">
              <b-form-input type="text" v-model="post.name" :class="{ 'is-invalid': validation.title }"
                placeholder="masukkan title post">
              </b-form-input>
              <div v-if="validation.title" class="mt-2">
                <b-alert show variant="danger">{{ validation.title[0] }}</b-alert>
              </div>
            </b-form-group>
            <b-form-group label="Content Post">
              <b-form-textarea id="textarea" v-model="post.content" :class="{ 'is-invalid': validation.title }"
                placeholder="masukkan content post" rows="5">
              </b-form-textarea>
              <div v-if="validation.content" class="mt-2">
                <b-alert show variant="danger">{{ validation.content[0] }}</b-alert>
              </div>
            </b-form-group>
            <b-button type="submit" variant="primary">UPDATE</b-button>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {

    data() {
      return {
        //state post
        post: {
          title: '',
        },
        //state validation
        validation: []
      }
    },

    mounted() {

      this.$axios.get(`https://gorest.co.in/public/v2/users/100/${this.$route.params.id}`)
        .then(response => {
            this.post.title   = response.data.data.title,
            this.post.content = response.data.data.content
        })
    },

    methods: {

      async update(e) {
        e.preventDefault()

        await this.$axios.put(`https://gorest.co.in/public/v2/users/100/${this.$route.params.id}`, {

           id:this.post.id,
             name: this.post.name,
            email: this.post.email,
             gender: this.post.gender,
             status: this.post.status

          })
          .then(() => {
            
            this.$router.push({
              name: 'post'
            })

          })
          .catch(error => {

            //assign error validasi  
            this.validation = error.response.data
          })
      }

    }

  }
</script>

<style>

</style>