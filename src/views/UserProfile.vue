<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{ state.user.username }}</h1>
            <div class="user-profile__admin-badge" v-if="state.user.isAdmin">
                Admin
            </div>
            <div class="user-profile__follower-count">
                <strong>Followers: {{ state.followers }}</strong>
            </div>
            <CreateTweetPanel @add-tweet="addTweet" />
        </div>
        <div class="user-profile__tweets-wrapper">
            <TweetItem
                v-for="tweet in state.user.tweets"
                :key="tweet.id"
                :username="state.user.username"
                :tweet="tweet"
            />
        </div>
    </div>
</template>

<script>
import { reactive, computed } from 'vue'
import { useRoute } from 'vue-router'
import { users } from '@/assets/users.js'
import TweetItem from '@/components/TweetItem.vue'
import CreateTweetPanel from '@/components/CreateTweetPanel.vue'

export default {
    name: 'UserProfile',
    components: {
        TweetItem,
        CreateTweetPanel
    },
    setup() {
        const route = useRoute()
        const userId = computed(() => route.params.userId)

        const state = reactive({
            followers: 0,
            user: users[userId.value - 1] || users[0]
        })

        function addTweet(tweet) {
            state.user.tweets.unshift({
                id: state.user.tweets.length + 1,
                content: tweet
            })
        }

        return { state, addTweet, userId }
    }
}
</script>

<style lang="scss" scoped>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    grid-gap: 50px;
    padding: 50px 5%;

    .user-profile__user-panel {
        display: flex;
        flex-direction: column;
        padding: 20px;
        background-color: white;
        border-radius: 5px;
        border: 1px solid $paleWhite;
        margin-bottom: auto;

        h1 {
            margin: 0;
        }

        .user-profile__admin-badge {
            background: rebeccapurple;
            color: white;
            border-radius: 5px;
            margin-right: auto;
            padding: 0 10px;
            font-weight: bold;
        }
    }

    .user-profile__tweet-wrapper {
        display: grid;
        grid-gap: 10px;
        margin-bottom: auto;
    }
}
</style>
