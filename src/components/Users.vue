<template>
    <v-layout row wrap>
        <v-flex xs10 sm8 md3 :key="user._id" v-for="user in userList">
            <UserCard :userObject="user" v-on:deleteUser="$emit('deleteUser', user._id)" v-on:userUpdated="$emit('userUpdated')"/>
        </v-flex>
    </v-layout>
</template>

<script>
import UserCard from './UserCard'

export default {
    name: 'Users',
    components: {
        UserCard
    },
    props: [
        'userList'
    ]
}
</script>