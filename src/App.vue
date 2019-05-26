<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Vuetify</span>
        <span class="font-weight-light">MATERIAL DESIGN</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
      >
        <span class="mr-2">Latest Release</span>
      </v-btn>
    </v-toolbar>

    <v-content>
      <v-layout row>
    <v-flex xs12 sm6 offset-sm3>
      <v-form
          ref="form"
          v-model="valid"
          lazy-validation
      >
      <v-text-field
      v-model="data.id"
      label="id"
      disabled
      ></v-text-field>

      <v-text-field
      v-model="data.name"
      :counter="30"
      :rules="nameRules"
      label="Name"
      required
      ></v-text-field>

      <v-text-field
      v-model="data.description"
      :rules="descRules"
      label="Description"
      required
      ></v-text-field>
      <v-layout row>
      
      <v-btn
      :disabled="!valid"
      color="success"
      @click="validate"
      >
      Insert
      </v-btn>

      <v-btn
      :disabled="data.id==''"
      color="warning"
      @click="update"
      >
      <i class="material-icons">
      create
      </i>
      </v-btn>
      <v-btn
      color="error"
      @click="del">
      <i class="material-icons">
      delete
      </i>
      </v-btn>

      <v-btn
      :disabled="data.name == ''"
      color="error"
      @click="reset"
      >
      Reset
      </v-btn>
      </v-layout>
    </v-form>
  </v-flex>
  </v-layout>
      <v-layout row>
        <v-flex xs12 sm6 offset-sm3>
          <v-card>
            <v-list two-line subheader>
              <v-list-tile avatar v-for="item in items">
                <v-list-tile-content @click="select($event, item)">
                  <v-list-tile-title>
                    {{item.name}}
                  </v-list-tile-title>
                  <v-list-tile-sub-title>{{item.description}}</v-list-tile-sub-title>
                </v-list-tile-content>
              </v-list-tile>
            </v-list>
          </v-card>
        </v-flex>
    </v-layout>
    </v-content>
  </v-app>
</template>

<script>
import Form from './components/Form.vue'
import FormList from './components/FormList.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Form,
    FormList
  },
  data () {
    return {
      data: {
        id: '',
        name: '',
        description: ''
      },
      items: [],
      valid: true,
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 30) || 'Name must be less than 30 characters'
      ],
      descRules: [
        v => !!v || 'Desc is required'
      ]
    }
  },
  methods: {
    validate () {
      if (this.$refs.form.validate()) {
        this.snackbar = true

        axios
        .post('http://localhost:3000/nota', this.data)
        .then(response => {
          console.log(response.data.data)
          if(response.data.data.affectedRows === 1)
          {
            this.items.push(this.data)
            this.data = {}
          }

          })

      }
    },
    reset () {
      this.data = {}
      this.$refs.form.reset()
    },
    del (){
      if(this.data.id)
      {
        console.log(this.data)
        axios
        .post(`http://localhost:3000/nota/delete/${this.data.id}`)
        .then(response => {
          
          if(response.status === 200)
          {
            const teste = (item, index, id)=>{
              console.log(id)
            }
            this.items.filter(teste(item, index, this.data.id))
          }

          })
      }
    },
    select(event, item){

      this.data = item

      console.log(item)
    },
    update(event){
      if(this.data.id)
      {
        axios
        .put('http://localhost:3000/nota', this.data)
        .then(response => {
          console.log(response)
          })
      }
    }
  },
  mounted () {

    axios
      .get('http://localhost:3000/nota')
      .then(response => {
        console.log(response.data.data)
        this.items = response.data.data
        })
  }
}
</script>