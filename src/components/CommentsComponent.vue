<template>
  <div class="media-body">
    <div class="card mb-3">
      <div class="card-body">
        <div class="container-md">
          <div class="row">
            <div class="col-1">
              <img v-bind:src="snippet.topLevelComment.snippet.authorProfileImageUrl" alt="Avatar"
                   class="rounded-circle mr-3 align-self-start">
            </div>
            <div class="col-9 align-self-start">
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

    <span v-if="replies != null">
                <div class="media mt-3">
                  <span v-for="(comments, index) in replies" v-bind:key="index">
                    <NestedComments
                        v-for="{snippet, index} in comments" v-bind:key="index"
                        :snippet="snippet"
                    ></NestedComments>
                  </span>
                </div>
            </span>
  </div>

</template>

<script>
import NestedComments from "./NestedComments.vue";

export default {
  name: "CommentsComponent",
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
    }
  }
}
</script>

<style scoped>

</style>