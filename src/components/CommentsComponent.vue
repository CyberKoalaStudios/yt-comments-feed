<template>
  <div class="media-body shadow p-3 mb-5 bg-white rounded">
    <div class="mb-3">
      <div class="">
        <div class="card-body">
          <div class="container-fluid">
            <div class="row">
<!--              <div class="col">-->
<!--                <img :src="snippet.topLevelComment.snippet.authorProfileImageUrl" alt="Avatar"-->
<!--                     class="rounded-circle mr-3 align-self-start ">-->
<!--              </div>-->
              <div class="col align-self-start">
                <div class="row">
                  <a :href="snippet.topLevelComment.snippet.authorChannelUrl">{{
                      snippet.topLevelComment.snippet.authorDisplayName
                    }}</a> · {{ formatDate(snippet.topLevelComment.snippet.updatedAt) }}
                </div>
                <div class="row">
                  <span class="card-text text-left" v-html="snippet.topLevelComment.snippet.textDisplay"></span>

                </div>

                <Transition>
                  <div v-if="show && replies!= null">
                    <hr>
                    <div class="media mt-3 ">
                    <span v-for="(comments, index) in replies" v-bind:key="index">
                      <NestedComments
                          v-for="{snippet, index} in comments.slice().reverse()" v-bind:key="index"
                          :snippet="snippet"
                      ></NestedComments>
                    </span>
                    </div>
                  </div>
                </Transition>
              </div>
              <div class="col-3">
                <!-- 16:9 aspect ratio -->
                <div class="embed-responsive embed-responsive-16by9">
                  <iframe class="embed-responsive-item"
                          :src="'https://www.youtube.com/embed/'+ snippet.topLevelComment.snippet.videoId + '?controls=0'"></iframe>
                </div>
<!--                  <button v-if="replies!= null" @click="show = !show" type="button" class="btn btn-secondary" data-toggle="collapse" data-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">Ответы</button>-->
                  <div class="custom-control custom-switch">
                    <input type="checkbox" :checked="show" class="custom-control-input" id="customSwitch1" @change="show = !show" >
                    <label class="custom-control-label" for="customSwitch1">Ответы</label>
                  </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import NestedComments from "./NestedComments.vue";

export default {
  inject: ['commentsThreads'],
  name: "CommentsComponent",

  data() {
    return {
      show: true
    }
  },

  props: {
    snippet: null,
    replies: null
  },

  components: {
    NestedComments,
  },

  methods: {
    truncate(v) {
      const newline = v.indexOf('\n')
      return newline > 0 ? v.slice(0, newline) : v
    },

    formatDate(v) {
      return v.replace(/T|Z/g, ' ')
    },
  },

}
</script>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

button {
  margin: 1em;
}

</style>