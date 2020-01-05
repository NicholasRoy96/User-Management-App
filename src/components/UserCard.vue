<template>
    <v-container>
        <v-row>
            <v-col cols="12">
                <v-card hover shaped class='userItem pa-2 ma-2'>
                    <v-row justify="center">                        
                        <v-col cols="4">
                            <v-avatar class="mt-4 ml-5" color="blue lighten-1" size="80">
                                <span class="white--text headline">{{formattedName}}</span>
                            </v-avatar>
                        </v-col>
                        <v-col cols="8">
                             <div class="text-right">
                                <v-menu offset-y :close-on-content-click="false" v-model='userMenu'>
                                    <template #activator="{ on }">
                                        <v-icon v-on="on">more_vert</v-icon>
                                    </template>
                                    <DeleteButton v-on:deleteUser="$emit('deleteUser', userObject._id)" @closeDeleteUser='userMenu = false'/>  
                                    <InfoButton :userId="userObject._id" v-on:userUpdated="$emit('userUpdated')" @closeEditUser='userMenu = false'/>
                                </v-menu>
                            </div>
                            <div>
                                <v-card-title class="justify-left">{{userObject.name}}</v-card-title>
                                <v-card-subtitle class="justify-left">{{userObject.email}}</v-card-subtitle>
                                <v-card-text class="justify-left">{{userObject.role}}</v-card-text>
                            </div>
                        </v-col>
                    </v-row>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
import DeleteButton from './DeleteButton'
import InfoButton from './InfoButton'

export default {
    name: 'UserItem',
    components: {
        DeleteButton,
        InfoButton
    },
    props: [
        'userObject'
    ],
    data() {
        return {
            userMenu: false
        }
    },
    computed: {
        formattedName() {
            const letterArray = this.userObject.name.split('')
            return letterArray[0].toUpperCase()
        }
    }
}
</script>

<style scoped>
    .userItem {
    background-color: #ffffff;
    padding: 10px;
    border-bottom: 1px #ccc dotted;
}

    .del {
    background: #ff0000;
    color: #fff;
    border: none;
    padding: 5px 9px;
    border-radius: 50%;
    cursor: pointer;
    float: right;
}

    .center {
    text-align: center;
}
</style>