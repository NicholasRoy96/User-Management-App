<template>
  <div>
    <v-btn
      class='mr-7 mt-7'
      outlined
      color='indigo'
      rounded
      @click="dialog = true"
    >
    Create New User
    <v-icon small right>add</v-icon>
    </v-btn>

    <v-dialog
      v-model="dialog"
      max-width="500"
    >
      <v-card>
        <h1 v-if="error">{{error}}</h1>
        <v-card-title class="headline">Enter the new user details</v-card-title>

  <v-form ref="form" v-model="valid">
    <v-container>
      <v-row>
        <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            outlined
            v-model="name"
            :rules="nameRules"
            :counter="10"
            label="Name"
            required
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            outlined
            v-model="email"
            :rules="emailRules"
            label="E-mail"
            required
          ></v-text-field>
        </v-col>
        <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            outlined
            v-model="role"
            :rules="roleRules"
            label="Job Role"
            required
          ></v-text-field>
        </v-col>
      </v-row>
    </v-container>
  </v-form>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            color="red darken-1"
            text
            @click="dialog=false"
          >
            Cancel
          </v-btn>
          
          <v-btn
            color="green darken-1"
            text
            @click="createUser()"
          >
            Create
          </v-btn>

        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-snackbar v-model="successMessage" color="success">
      New User Created!
      <v-btn @click="successMessage=false">
        <v-icon small color="success">close</v-icon>
      </v-btn>
    </v-snackbar>
  </div>
</template>

<script>
  import axios from 'axios'
  
  export default {
      name: 'CreateUserButton',
      data: () => ({
        error: null,
        successMessage: false,
        dialog: false,    
        valid: false,
        name: '',
        nameRules: [
            v => !!v || 'Name is required',
            v => v.length <= 10 || 'Name must be less than 10 characters',
        ],
        email: '',
        emailRules: [
            v => !!v || 'E-mail is required',
            v => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
        role: '',
        roleRules: [
          v => !!v || 'Job Role is required'
        ]
    }),
    methods: {
        async createUser() {
            try {
                await axios.post('http://localhost:3000/user/createUser', {
                    name: this.name,
                    email: this.email,
                    role: this.role
                })

                this.dialog = false
                this.name = ''
                this.email = ''
                this.role = ''
                this.$emit('userCreated')
                this.successMessage = true
            } catch (e) {
                this.error = e.response.data
            }
        }
    }
}
</script>