<template>
   <v-container fluid fill-height>
    <v-layout align-center justify-center>
      <v-flex xs12 sm8 md8>

        <v-card class="elevation-12"> 

          <v-card-title primary-title>
            <h3 class="headline mb-0">Add a new menu category
              <v-btn small color="primary"
                to="/menu-categories"
              >View menu categories</v-btn>
            </h3>
          </v-card-title> 

          <form 
            @submit.prevent="create"
            @keydown="form.errors.clear($event.target.name)"
          >

            <v-card-text> 

              <v-text-field prepend-icon="domain" name="name" label="Menu category" id="name" type="text" 
                ref="name"
                v-model="form.name"
                :rules="[() => !form.errors.has('name') || form.errors.get('name') ]"
              ></v-text-field>  

            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn type="submit" color="primary" 
              >
                Create Menu Category
              </v-btn>
            </v-card-actions>
            
          </form> 

        </v-card>

      </v-flex>
    </v-layout>
  </v-container>
</template>

<script type="text/javascript">
  
  import Form from 'helpers/Form.js'

  export default {

    data: () => ({
      form: new Form({
        name: '' 
      }),
      units: []
    }),

    mounted() {
      this.form.get('/api/menu-categories')
        .then(data  =>  {
            data.data.forEach(item => {
              this.units.push({
                text: item.unit,
                value: item.id
              })
            })
          })
    },

    methods: {

      create() {
        this.form.post('/api/menu-categories')
          .then(data  =>  {
            this.$router.push('/menu-categories');
          })
          .catch(errors => {
            this.form.validate(this.$refs)
          })
      }, 
    }
  }
</script>