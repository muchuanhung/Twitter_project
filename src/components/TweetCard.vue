<template>
  <div class="tweetCard-wrapper">
    <div class="card-left avatar" @click="linkedUser(tweetUser.id)">
      <img class="avatar" :src="tweetUser.avatar">
    </div>
    <div class="card-right">
      <div class="card-head">
        <h5 class="card-name" @click="linkedUser(tweetUser.id)">
          {{ tweetUser.name }}
        </h5>
        <h5 class="card-account">
          {{ tweetUser.account | accountTag }}
        </h5>
        <span>．</span>
        <h5 class="card-time">
          {{ createdAt | fromNow }}
        </h5>
      </div>
      <p>
        {{ description }}
      <p>
      <div class="card-foot">
        <img class="reply" @click="linkedReply(id)">
        <h6>{{ replysCount }}</h6>
        <img v-if="isLiked" @click="deleteLikes()" class="heart-active">
        <img v-else @click="addLikes()" class="heart">
        
        <h6>{{ likesCount }}</h6>
      </div>
    </div>
  </div>
</template>

<script>
import { fromNowFilter, accountTagFilter } from '../utils/mixins'
export default {
  mixins: [fromNowFilter, accountTagFilter],
  props: {
    tweetCard: {
      type: Object,
      required: true,
    }
  },
  data() {
    return {
      id: 0,
      tweetUser: {},
      description: '',
      createdAt: '',
      replysCount: 0,
      likesCount: 0,
      isLiked: false
    }
  },
  methods: {
    fetchTweetCard() {
      const { Likes, User, Replies, description, id, createdAt } = this.tweetCard
      this.id = id
      this.tweetUser = User
      this.description = description
      this.createdAt = createdAt
      this.replysCount = Replies.length
      this.likesCount = Likes.length
    },
    addLikes() {
      // todo: connect API
      this.isLiked = true
      this.likesCount++
    },
    deleteLikes() {
      // todo: connect API
      this.isLiked = false
      this.likesCount--
    },
    linkedUser(userId) {
      this.$router.push({ name: 'user', params: { id: userId }})
    },
    linkedReply(tweetId) {
      this.$router.push({ name: 'tweet', params: { id: tweetId }})
    }
  },
  created() {
    this.fetchTweetCard()
  }
}
</script>