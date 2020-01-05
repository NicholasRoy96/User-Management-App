<template>
  
  <v-row justify="center">
    <v-btn
      class='editUser'
      text rounded
      color="blue lighten-2"
      @click="openInfo()"
    >
    <v-icon>edit</v-icon>
    </v-btn>

    <v-dialog
      v-model="openEditUser"
      max-width="500"
    >
      <v-card>
        <v-card-title class="headline">User: {{user.name}}</v-card-title>
        <v-card-subtitle> 
          This user's email is {{user.email}}
        </v-card-subtitle>

        <v-form ref="form" v-model="valid">
          <v-container>
            <v-row>
              <v-col
                cols="12"
                md="4"
              >
                <v-text-field
                  outlined
                  v-model="user.name"
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
                  v-model="user.email"
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
                  v-model="user.role"
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
            @click="closeEditUser()"
          >
            Cancel
          </v-btn>
          
          <v-btn
            color="green darken-1"
            text
            @click="updateUser()"
          >
            Update
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
  import axios from 'axios'
  
  export default {
      name: 'InfoButton',
      props: [
          'userId'
      ],
      data () {
        return {
            valid: true,
            user: {},
            openEditUser: false,
            nameRules: [
                v => !!v || 'Name is required',
                v => v.length <= 10 || 'Name must be less than 10 characters',
            ],
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+/.test(v) || 'E-mail must be valid',
            ],
            roleRules: [
              v => !!v || 'Job Role is required'
            ]
        }      
    },
    methods: {
        async getUserInfo(userId) {
            const result = await axios.get(`http://localhost:3000/user/getUser/${userId}`)
            this.user = result.data
        },
        async openInfo() {
            await this.getUserInfo(this.userId)
            this.openEditUser = true
        },
        closeEditUser() {
          this.openEditUser=false
          this.$emit('closeEditUser')
        },
        async updateUser() {
            try {
                const {name, email, role} = this.user
                await axios.put(`http://localhost:3000/user/updateUser/${this.userId}`, {
                    name,
                    email,
                    role
                })
                this.openEditUser = false
                this.$emit('userUpdated')
            } catch (e) {
                this.error = e.response.data
            }
        }
    }
}
</script>

<style scoped>
.editUser {
  background-color: white;
}
</style>