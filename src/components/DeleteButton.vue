<template>
  <v-row justify="center">
    <v-btn
      class='deleteUser'
      text rounded
      color="red darken-1"
      @click="openDeleteUser()"
    >
    <v-icon>delete</v-icon>
    </v-btn>

    <v-dialog
      v-model="showDeleteUser"
      max-width="290"
    >
      <v-card>
        <v-card-title class="headline">Are you sure you want to delete the user?</v-card-title>

        <v-card-text>
          This action cannot be undone.
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            color="red darken-1"
            text
            @click="closeDeleteUser()"
          >
            No
          </v-btn>
          
          <v-btn
            color="green darken-1"
            text
            @click="deleteUser()"
          >
            Yes
          </v-btn>
          
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
  export default {
      name: 'DeleteButton',
      data () {
      return {
        showDeleteUser: false,
      }
    },
    methods: {
      openDeleteUser() {
        this.showDeleteUser = true
      },
      closeDeleteUser() {
        this.showDeleteUser = false
        this.$emit('closeDeleteUser')
      },
      deleteUser() {
        this.$emit('deleteUser')
        this.closeDeleteUser()
      }
    }
}
</script>

<style scoped>
.deleteUser {
  background-color: white;
}
</style>