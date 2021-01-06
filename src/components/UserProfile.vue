<template>
    <div class="user-profile">
        <div class="user-profile_user-panel">
            <h1 class="user-profile_username">@{{ user.username }}</h1>
            <div class="user-profile_admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <div class="user-profile_follower-count">
                <strong>Followers: </strong> {{ followers }}
            </div>
            <form class="user-profile_create-twoot" @submit.prevent="createNewTwoot" :class="{'exceeded': newTwootCharacterCount > 200}">
                <label for="newTwoot"><strong>New Twoot</strong> ({{ newTwootCharacterCount }}/200) </label>
                <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>

                <div class="user-profile_create-twoot-type">
                    <label for="newTwootType"><strong>Type: </strong></label>
                    <select id="newTwootType" v-model="selectedTwootType">
                        <option :value="option.value" v-for="option in twootTypes" :key="option.id">
                            {{ option.name }}
                        </option>
                    </select>
                </div>

                <button>
                    Twoot
                </button>
            </form>
        </div>
        <div class="user-profile_twoots-wrapper">
            <TwootItem 
                v-for="twoot in user.twoots" 
                :key="twoot.id" 
                :username="user.username" 
                :twoot="twoot" 
                @favourite="toggleFavourite" 
            />
        </div>
    </div>
</template>

<script>
import TwootItem from "./TwootItem";

export default {
    name: 'UserProfile',
    components: {
        TwootItem
    },
    data(){
        return{
            newTwootContent: '',
            selectedTwootType: 'instant',
            twootTypes: [
                {
                    value: 'draft', name: 'Draft',
                },
                {
                    value: 'instant', name: 'Instant Twoot',
                }
            ],
            followers: 0,
            followed: 10,
            user:{
                id: 1,
                username: 'thechrisdev',
                firstName: 'Chris',
                lastName: 'Miracle',
                email: 'mcchris54@yahoo.com',
                isAdmin: true,
                twoots:[
                    {
                        id: 1,
                        content: 'Twotter is not amazing!',
                    },
                    {
                        id: 2,
                        content: "Don't forget to follow @thechrisdev!"
                    }
                ]
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
        },
        newTwootCharacterCount(){
            return this.newTwootContent.length;
        }
    },
    methods:{
        followUser(){
            this.followers++
        },
        toggleFavourite(id){
            console.log(`Favourited Tweet #${id}`)
        },
        createNewTwoot(){
            if(this.newTwootContent && this.selectedTwootType != 'draft'){
                this.user.twoots.unshift({
                    id: this.user.twoots.length + 1,
                    content: this.newTwootContent
                })
                this.newTwootContent = '';
            }
        },
        mounted(){
        this.followUser();
        }
    }
    };
</script>

<style scoped>
    .user-profile{
        display: grid;
        grid-template-columns: 1fr 3fr;
        width: 100%;
        padding: 50px 5%;
    }

    .user-profile_user-panel{
        display: flex;
        flex-direction: column;
        margin-right: 50px;
        padding: 20px;
        background-color: white;
        border: 1px solid #dfe3e8;
    }

    .user-profile_admin-badge{
        background: purple;
        color: white;
        border-radius: 5px;
        margin-right: auto;
        padding: 0 10px;
        font-weight: bold;
    }

    h1{
        margin: 0;
    }

    .user-profile_twoot-wrapper{
        display: grid;
        grid-gap: 10px;
    }

    .exceeded{
        color: red;
        border-color: red;
    }

    .user-profile_create-twoot{
        padding-top: 20px;
        display: flex;
        flex-direction: column;
    }
</style>
