<template>
  <h1>Latest Comments</h1>
  <div class="form">

    <ul class="list-unstyled">
      <template v-if="commentsThreads.items != null">
        <li class="media" v-for="{id, snippet, replies} in commentsThreads.items" v-bind:key="id">
          <img v-bind:src="snippet.topLevelComment.snippet.authorProfileImageUrl" alt="Avatar"
               class="rounded-circle mr-3 align-self-start">
          <div class="media-body">
            <div class="card mb-3">
              <div class="card-body">
                <div class="container-md">
                  <div class="row">
                    <!--                <div class="col">-->
                    <!--                </div>-->
                    <div class="col-8 align-self-start">
                      <div class="row">
                        <a href="{{snippet.topLevelComment.snippet.authorChannelUrl}}">{{
                            snippet.topLevelComment.snippet.authorDisplayName
                          }}</a> · {{ formatDate(snippet.topLevelComment.snippet.updatedAt) }}
                      </div>
                      <p class="card-text text-left">{{ truncate(snippet.topLevelComment.snippet.textDisplay) }}</p>
                    </div>
                    <div class="col">
                      <a href="https://www.youtube.com/watch?v={{ snippet.topLevelComment.snippet.videoId }}">URL</a>
                    </div>

                  </div>

                </div>
              </div>
            </div>

            <div class="media mt-3">
              <a class="mr-3" href="#">
                <!--              <img src="..." alt="...">-->
              </a>
              <div class="media-body">
                <!--              <h5 class="mt-0">Media heading</h5>-->
                <!--              <p>{{ replies }}</p>-->

                <span v-for="(comments, snippet, index) in replies" v-bind:key="index">
<!--                    {{ snippet.textDisplay }} {{index}}-->
                  {{comments}}

                </span>
                <!--              <ul class="list-unstyled">-->

                <!--                <li v-for="{comments, index} in replies" v-bind:key="index">-->
                <!--                  <p>{{comments.snippet.textDisplay}}</p>-->
                <!--                </li>-->


                <!--              </ul>-->
              </div>
            </div>

          </div>

        </li>
      </template>
    </ul>
  </div>
</template>

<script>
import commentsData from '../mock/comments.json';

const API_URL = `https://www.googleapis.com/youtube/v3/commentThreads?key=AIzaSyDN6xC9p6ZCxoxE9EtACanbVb6aVGcuqzk&part=snippet, id, replies&allThreadsRelatedToChannelId=UCUpVfgd42h7pwZwCTcwjp8g`

export default {
  name: "FetchComments",

  data: () => ({
    // pageInfo: ['main', 'v2-compat'],
    commentsThreads: {},
    commentsMock: commentsData,
    maxResults: 20
  }),

  created() {
    // fetch on init
    this.fetchData()
  },

  watch: {
    // re-fetch whenever currentBranch changes
    // currentBranch: 'fetchData',
    maxResults: 'fetchData'
    // commentsThreads: 'fetchData'
  },

  computed: {},

  methods: {
    async fetchData() {
      const url = `${API_URL}&${this.maxResults}`
      this.commentsThreads = await (await fetch(url)).json()
    },

    truncate(v) {
      const newline = v.indexOf('\n')
      return newline > 0 ? v.slice(0, newline) : v
    },

    formatDate(v) {
      return v.replace(/T|Z/g, ' ')
    }
  },

  // This function will be called when the component is mounted.
  mounted() {
    console.log(`The initial count is ${this.commentsThreads}.`)
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

/*ul {*/
/*  list-style-type: none;*/
/*  padding: 0;*/
/*  margin: 0;*/
/*}*/

.form {
  /*margin: 1em;*/
  padding: 2.5em;
}

.row {
  background: rgba(255, 0, 0, .1)
}

.row > div {
  padding: 10px 15px;
  background: rgba(86, 61, 124, .15);
  border: 1px solid rgba(86, 61, 124, .2)
}

.column > div {
  padding: 10px 15px;
  background: rgba(86, 61, 124, .15);
  border: 1px solid rgba(86, 61, 124, .2)
}
</style>