<template>
<!--  <h1>Последние комментарии</h1>-->
  <div class="form">
    <div class="d-flex justify-content-center">
      <div class="spinner-grow text-primary" role="status" v-if="loading">
        <span class="visually-hidden"></span>
      </div>
    </div>

    <select class="form-select mb-3" aria-label="Канал" @change="onChannelChange($event)" v-model="selectedChannel">
      <option value="coolday" selected>Канал COOLDAY</option>
      <option value="cyberkoala">Канал CyberKoala</option>
    </select>

    <ul class="list-unstyled" v-if="!loading">
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
        <li class="page-item"><a class="page-link" type="button" @click="nextPage">Следующая</a></li>
      </ul>
    </nav>

  </div>
</template>

<script>
import commentsData from '../mock/comments.json';
import CommentsComponent from "./CommentsComponent.vue";
import {toRaw} from "vue";

const CYBERKOALA_ID = `UCfEBnO2ZWyDHnN6bQSSlg1w`
const COOLDAY_ID = `UCUpVfgd42h7pwZwCTcwjp8g`
const API_URL = `https://www.googleapis.com/youtube/v3/commentThreads?key=AIzaSyDN6xC9p6ZCxoxE9EtACanbVb6aVGcuqzk&part=snippet, id, replies&allThreadsRelatedToChannelId=`


export default {
  name: "FetchComments",

  data() {
    return {
      // pageInfo: ['main', 'v2-compat'],
      commentsThreads: {},
      commentsMock: commentsData,
      commentItems: [],
      maxResults: 10,
      pageToken: String,
      firstPageUrl: String,
      pageTokens: [],
      loading: Boolean,
      selectedChannel: "coolday",
      channelId: COOLDAY_ID
    }
  },

  components: {
    CommentsComponent,
  },

  props: {},

  created() {

    // fetch on init
    this.fetchData()

    // setInterval(function () {
    //   this.fetchData();
    // }.bind(this), 30000);
  },

  watch: {
    // re-fetch whenever maxResults changes
    maxResults: 'fetchData',
    // pageToken: 'nextPage'
    // selectedChannel: ''
  },

  computed: {
  },

  provide() {
    // use function syntax so that we can access `this`
    return {
      commentsThreads: this.commentsThreads
    }
  },

  methods: {
    async fetchData() {
      const url = `${API_URL}${this.channelId}&maxResults=${this.maxResults}`
      this.firstPageUrl = url
      this.commentsThreads = await (await fetch(url).then(response => {
        while(!response.ok){
          this.loading = true
        }
        if(response.ok){
          this.loading = false
          return response.json();
        }
        throw new Error(response.statusText);
      }))

      // this.pageTokens.push(this.commentsThreads.nextPageToken);
      this.pageToken = this.commentsThreads.nextPageToken;

      // while (this.pageToken != null) {
      //   await this.nextPage();
      //   console.log(this.commentItems);
      // }
    },

    async nextPage() {
      const url = `${API_URL}${this.channelId}&maxResults=${this.maxResults}&pageToken=${this.pageToken}`

      this.commentsThreads = await (await fetch(url)).json()
      const items = toRaw(this.commentsThreads.items)

      this.commentItems.push(items)
      // this.pageTokens.push(this.commentsThreads.nextPageToken);
      this.pageToken = this.commentsThreads.nextPageToken
    },

    async loadPage(index) {
      const url = `${API_URL}${this.channelId}&maxResults=${this.maxResults}&pageToken=${this.pageTokens[index]}`

      this.commentsThreads = await (await fetch(url)).json()
      // this.pageToken = this.commentsThreads.nextPageToken
    },

    async prevPage() {
      this.commentsThreads = await (await fetch(this.firstPageUrl)).json()
      this.pageToken = this.commentsThreads.nextPageToken
    },

    async onChannelChange(event) {
      if (event.target.value === 'coolday'){
        this.channelId = COOLDAY_ID
      } else {
        this.channelId = CYBERKOALA_ID
      }
      await this.fetchData()
      console.log(event.target.value)
    },

  },

  // This function will be called when the component is mounted.
  mounted() {
  },

  unmounted() {
  },



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