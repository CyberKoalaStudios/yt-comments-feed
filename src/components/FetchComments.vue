<template>
  <h1>Latest Comments</h1>
  <div class="form">

    <ul class="list-unstyled">
        <li v-for="{id, snippet, replies} in commentsThreads.items" v-bind:key="id" class="media">
<!--          <img alt="Avatar" class="rounded-circle mr-3 align-self-start"-->
<!--               v-bind:src="snippet.topLevelComment.snippet.authorProfileImageUrl">-->
          <CommentsComponent :snippet="snippet" :replies="replies"/>
        </li>
    </ul>
  </div>
</template>

<script>
import commentsData from '../mock/comments.json';
import CommentsComponent from "./CommentsComponent.vue";

const API_URL = `https://www.googleapis.com/youtube/v3/commentThreads?key=AIzaSyDN6xC9p6ZCxoxE9EtACanbVb6aVGcuqzk&part=snippet, id, replies&allThreadsRelatedToChannelId=UCUpVfgd42h7pwZwCTcwjp8g`

export default {
  name: "FetchComments",

  data() {
    return {
    // pageInfo: ['main', 'v2-compat'],
    commentsThreads: {},
    commentsMock: commentsData,
    maxResults: 40
  }},

  components: {
    CommentsComponent,

  },

  created() {
    // fetch on init
    this.fetchData()
  },

  watch: {
    // re-fetch whenever currentBranch changes
    // currentBranch: 'fetchData',
    maxResults: 'fetchData',
    // commentsThreads: 'fetchData'
  },

  computed: {},

  methods: {
    async fetchData() {
      const url = `${API_URL}&${this.maxResults}`
      this.commentsThreads = await (await fetch(url)).json()
    },

    // truncate(v) {
    //   const newline = v.indexOf('\n')
    //   return newline > 0 ? v.slice(0, newline) : v
    // },
    //
    // formatDate(v) {
    //   return v.replace(/T|Z/g, ' ')
    // }
  },

  // This function will be called when the component is mounted.
  mounted() {
    // console.log(`The initial count is ${this.commentsThreads}.`)
  }
}
</script>

<style scoped>
.avatar {
  vertical-align: middle;
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

.form {
  /*margin: 1em;*/
  padding: 2.5em;
}

.row {
  background: rgba(255, 0, 0, .1)
}

.row > div {
  /*padding: 10px 15px;*/
  background: rgba(86, 61, 124, .15);
  border: 1px solid rgba(86, 61, 124, .2)
}

.column > div {
  /*padding: 10px 15px;*/
  background: rgba(86, 61, 124, .15);
  border: 1px solid rgba(86, 61, 124, .2)
}
</style>