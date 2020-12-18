<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{ user.username }}</h1>
            <div class="user-profile__admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <div class="user-profile__follower-count">
                <strong>Followers: {{ followers }}</strong>
            </div>
        </div>
        <div class="user-profile__tweets-wrapper">
            <TweetItem
                v-for="tweet in user.tweets"
                :key="tweet.id"
                :username="user.username"
                :tweet="tweet"
                @favourite="toggleFavourite"
            />
        </div>
    </div>
</template>

<script>
import TweetItem from './TweetItem.vue'

export default {
    name: 'UserProfile',
    components: {
        TweetItem,
    },
    data() {
        return {
            followers: 0,
            user: {
                id: 1,
                username: '_dsabalete',
                firstName: 'David',
                lastName: 'sabalete',
                email: 'dsabalete@hotmail.com',
                isAdmin: true,
                tweets: [
                    {
                        id: 1,
                        content: 'Vue-twitter is amazing',
                    },
                    {
                        id: 2,
                        content: 'Do not forget to subscribe',
                    },
                ],
            },
        }
    },
    watch: {
        followers(newFollowerCount, oldFollowerCount) {
            if (oldFollowerCount < newFollowerCount) {
                console.log(`${this.user.username} has gained a follower`)
            }
        },
    },
    computed: {
        fullName() {
            return `${this.user.firstName} ${this.user.lastName}`
        },
    },
    methods: {
        followUser() {
            this.followers++
        },
        toggleFavourite(id) {
            console.log(`Favourite Tweet ${id}`)
        },
    },
    mounted() {
        this.followUser()
    },
}
</script>

<style scoped>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width: 100%;
    padding: 50px 5%;
}

.user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;
}

.user-profile__admin-badge {
    background: rebeccapurple;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
}

h1 {
    margin: 0;
}
</style>
