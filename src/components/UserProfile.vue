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
import { reactive } from 'vue'
import TweetItem from './TweetItem.vue'
import CreateTweetPanel from './CreateTweetPanel.vue'

export default {
    name: 'UserProfile',
    components: {
        TweetItem,
        CreateTweetPanel,
    },
    setup() {
        const state = reactive({
            followers: 0,
            user: {
                id: 1,
                username: 'owkenobi',
                firstName: 'Obi-Wan',
                lastName: 'Kenobi',
                email: 'owkenobi@hotmail.com',
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
        })

        function addTweet(tweet) {
            state.user.tweets.unshift({
                id: state.user.tweets.length + 1,
                content: tweet,
            })
        }

        return { state, addTweet }
    },
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
