<template>
  <div class="container">
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5 style="float:left;">DAFTAR PENGGUNA</h5>

          <b-button v-b-modal.modal-1 variant="primary" class="mb-3" style="float:right;">BUAT PENGGUNA</b-button>
          <b-table striped bordered hover :items="posts" :fields="fields" show-empty :current-page="currentPage"
        :per-page="0"></b-table>
        </b-card>
      </b-col>
    </b-row>
    <b-row>
      <b-modal id="modal-1">
      <b-col>
        <b-card class="shadow-md border-0 rounded-lg">
           <h5>BUAT POST</h5>
         
         <b-form @submit="store">
               <b-button type="submit" variant="primary">SIMPAN</b-button>
            <b-form-group label="Title Post">
              <b-form-input type="text" v-model="post.title" :class="{ 'is-invalid': validation.title }"
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
          </b-form>
        </b-card>
      </b-col>
      </b-modal>
    </b-row>
    <b-pagination
      first-text="First"
      prev-text="previous"
       next-text="Next"
      align="left"
      last-text="Last"
          v-model="currentPage"
          style="mt-20  ml-10"
          :total-rows="totalPages"
          :per-page="recordsPerPage">
          <template #prev-text>
<i class="fa  fa-arrow-right"></i>
       </template>
                 <template #prev-text>
<i class="fa  fa-arrow-left"></i>
       </template>
   </b-pagination>
  </b-container>
  </div>
</template>

<script>
  export default {

    data() {
      return {
      currentPage:1,
      totalPages:0,
      recordsPerPage:50,
      isLoading: false,
      fields: ['id ', 'name', 'email','gender','status'],
       post: {
           title: '',
           content: ''
        },
        posts: [],
        validation: []
      }
    },
    watch:{
      currentPage: {
        handler: function(value) {
          this.params = `page=${value}&size=${this.recordsPerPage}`
       }
      }
    },
    mounted() {

   
      this.$axios.get('https://gorest.co.in/public/v2/users/100')
        .then(response => {
          this.posts = response.data.data

        })
        .catch(error => {
          console.log(error.response.data)
        })
    },

    methods: {

       async deletePost(row) {
        
        await this.$axios.delete(`https://gorest.co.in/public/v2/users/100/${row.item.id}`)
          .then(() => {

            this.posts.splice(row.index, 1);

          })

      },
      async store(e) {
         e.preventDefault()
        await this.$axios.post('https://gorest.co.in/public/v2/public/v2/users', {
             title: this.post.title,
            content: this.post.content

            })
           .then(() => {
            
          this.$router.push({
               name: 'post'
             })
           })
           .catch(error => {
             this.validation = error.response.data
          })
      }
    }

  }
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
