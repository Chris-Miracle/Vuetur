<template>
  <div id="app">
    @{{ user.username }} - {{ fullName}} <!--- {{ Interactions}} -->
    <strong>Followers: </strong> {{ followers }}
    <button v-on:click="followUser">
      Follow
    </button>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      followers: 0,
      followed: 10,
      user:{
        id: 1,
        username: 'thechrisdev',
        firstName: 'Chris',
        lastName: 'Miracle',
        email: 'mcchris54@yahoo.com',
        isAdmin: true
      }
    }
  },
  watch:{
    followers(newFollowerCount, oldFollowerCount){
      if(oldFollowerCount < newFollowerCount){
        console.log(`${this.user.username} has gained a follower!`)
      }
    }
  },
  computed: {
    fullName(){
      return `${this.user.firstName} ${this.user.lastName}`;
    },
    Interactions(){
      return `${this.followers} ${this.followed}`;
    }
  },
  methods:{
    followUser(){
      this.followers++
    }
  },
  mounted(){
    this.followUser();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  background-color: #f3f5fa;
}
</style>
