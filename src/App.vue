<template>
  <v-app id="app">
    <nav>
    <v-app-bar flat color='indigo lighten-4' height='73'>
      <v-row justify="center" align="center">
        <v-img
        src="https://www.pastepic.xyz/images/2019/12/16/36e78d84-e2ad-4cd6-a831-185efbdd76ab_200x200f294652af2c4d28d.png"
        aspect-ratio="1"
        max-width="200"
        max-height="100"
      ></v-img>
      </v-row>
  </v-app-bar>

  <!-- Nav drawer -->

  <v-navigation-drawer app permanent left width='70'>
      <template v-slot:prepend>
        <v-list-item two-line>
          <v-list-item-avatar>
            <img src="https://www.pastepic.xyz/images/2019/12/16/NickGravatar48b52d96243baee5.jpg">
          </v-list-item-avatar>
        </v-list-item>
      </template>
      <v-divider></v-divider>
      <v-list>
        <v-list-item-group color="primary">
          <v-list-item @click="openWarningSnackbar(item.message)" v-for="(item, i) in [{icon: 'person', message: 'Account Details'}, {icon: 'account_circle', message: 'Member Area'}]" :key="i">      
            <v-list-item-icon>
              <v-icon>{{item.icon}}</v-icon>
            </v-list-item-icon>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
  </nav>

<!-- Create user button -->

    <v-content>
      <v-row justify='end'>
          <CreateUserButton @userCreated="getUsers"/>      
      </v-row>
      <v-row>
        <v-col cols='10' offset='1'>
          <Users :userList="users" @deleteUser="deleteUser" @userUpdated="updatedUser"/>
        </v-col>
      </v-row>
    </v-content>

<!-- Snackbars for main page -->

    <v-snackbar v-model="warningSnackbar" color="warning">
      {{warningSnackbarMessage}} is not yet available
      <v-btn @click="warningSnackbar=false">
        <v-icon small color="warning">close</v-icon>
      </v-btn>
    </v-snackbar>

    <v-snackbar v-model="successSnackbar" color="success">
      User Updated!
      <v-btn @click="successSnackbar=false">
        <v-icon small color="success">close</v-icon>
      </v-btn>
    </v-snackbar>
    
    <v-snackbar v-model="deleteSnackbar" color="success">
      User Deleted!
      <v-btn @click="deleteSnackbar=false">
        <v-icon small color="success">close</v-icon>
      </v-btn>
    </v-snackbar>
    
  </v-app>
</template>

<script>
import axios from 'axios'
import Users from './components/Users'
import CreateUserButton from './components/CreateUserButton'

export default {
  name: 'app',
  components: {
    Users,
    CreateUserButton
  },
  data: function() {
    return {
       users: [],
       warningSnackbarMessage: '',
       warningSnackbar: false,
       successSnackbar: false,
       deleteSnackbar: false
    }
  },
  async mounted() {
    await this.getUsers()
  },
  methods: {
    async getUsers() {
      const result = await axios.get('http://localhost:3000/user/getAllUsers/')
      this.users = result.data
    },
    async deleteUser(_id) {
      await axios.delete(`http://localhost:3000/user/deleteUser/${_id}`)
      await this.getUsers()
      this.openDeleteSnackbar()
    },
    openWarningSnackbar(message) {
      this.warningSnackbar = true
      this.warningSnackbarMessage = message
    },
    openDeleteSnackbar() {
      this.deleteSnackbar = true
    },
    openSuccessSnackbar() {
      this.successSnackbar = true
    },
    async updatedUser() {
      await this.getUsers()
      this.openSuccessSnackbar()
    }
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  #app {
    background-color: #fafcfb
  }

  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }
</style>