<template>
<!--  <h1>Последние комментарии</h1>-->
  <div class="form">

    <ul class="list-unstyled">
      <li v-for="{id, snippet, replies} in commentsThreads.items" v-bind:key="id" class="media">
        <!--          <img alt="Avatar" class="rounded-circle mr-3 align-self-start"-->
        <!--               v-bind:src="snippet.topLevelComment.snippet.authorProfileImageUrl">-->
        <CommentsComponent :snippet="snippet" :replies="replies"/>
      </li>
    </ul>

    <nav aria-label="Page nav">
      <ul class="pagination justify-content-center">
        <li class="page-item"><a class="page-link" @click="prevPage">Первая</a></li>
        <li v-for="(page, index) in pageTokens" v-bind:key="index" class="page-item"><a class="page-link" @click="loadPage(index)">{{index}}</a></li>
<!--                <li class="page-item"><a class="page-link" href="#">1</a></li>-->
<!--                <li class="page-item"><a class="page-link" href="#">2</a></li>-->
<!--                <li class="page-item"><a class="page-link" href="#">3</a></li>-->
        <li class="page-item"><a class="page-link" type="button" @click="nextPage">Следующая</a></li>
      </ul>
    </nav>

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
      maxResults: 10,
      pageToken: String,
      firstPageUrl: String,
      pageTokens: []
    }
  },

  components: {
    CommentsComponent,
  },

  props: {},

  created() {
    // fetch on init
    this.fetchData()
  },

  watch: {
    // re-fetch whenever maxResults changes
    maxResults: 'fetchData',
  },

  computed: {},

  provide() {
    // use function syntax so that we can access `this`
    return {
      commentsThreads: this.commentsThreads
    }
  },

  methods: {
    async fetchData() {
      const url = `${API_URL}&maxResults=${this.maxResults}`
      this.firstPageUrl = url
      this.commentsThreads = await (await fetch(url).then(response => {
        if(response.ok){
          return response.json();
        }
        throw new Error(response.statusText);
      })
      )
      this.pageTokens.push(this.commentsThreads.nextPageToken);
      this.pageToken = this.commentsThreads.nextPageToken
    },

    async nextPage() {
      const url = `${API_URL}&maxResults=${this.maxResults}&pageToken=${this.pageToken}`

      this.commentsThreads = await (await fetch(url)).json()
      this.pageTokens.push(this.commentsThreads.nextPageToken);
      this.pageToken = this.commentsThreads.nextPageToken
    },

    async loadPage(index) {
      const url = `${API_URL}&maxResults=${this.maxResults}&pageToken=${this.pageTokens[index]}`

      this.commentsThreads = await (await fetch(url)).json()
      // this.pageToken = this.commentsThreads.nextPageToken
    },

    async prevPage() {
      this.commentsThreads = await (await fetch(this.firstPageUrl)).json()
      this.pageToken = this.commentsThreads.nextPageToken
    }
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
  background-color: #fffcf7;
  color: #070707;
  /*font-family: Inter;*/
  padding: 1.5em;
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