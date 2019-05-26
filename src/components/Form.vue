<template>
    <v-layout row>
    <v-flex xs12 sm6 offset-sm3>
        <v-form
            ref="form"
            v-model="valid"
            lazy-validation
        >
            <v-text-field
            v-model="name"
            :counter="30"
            :rules="nameRules"
            label="Name"
            required
            ></v-text-field>

            <v-text-field
            v-model="desc"
            :rules="descRules"
            label="Description"
            required
            ></v-text-field>

            <v-btn
            :disabled="!valid"
            color="success"
            @click="validate"
            >
            Validate
            </v-btn>

            <v-btn
            color="error"
            @click="reset"
            >
            Reset Form
            </v-btn>
        </v-form>
 </v-flex>
  </v-layout>
</template>

<script>
  export default {
    data: () => ({
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 30) || 'Name must be less than 30 characters'
      ],
      desc: '',
      descRules: [
        v => !!v || 'Desc is required'
      ]
    }),

    methods: {
      validate () {
        if (this.$refs.form.validate()) {
          this.snackbar = true
        }
      },
      reset () {
        this.$refs.form.reset()
      }
    }
  }
</script>