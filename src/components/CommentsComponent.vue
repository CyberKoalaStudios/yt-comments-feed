<template>
  <div class="media-body shadow p-3 mb-5 bg-white rounded">
    <div class="mb-3">
      <div class="">
        <div class="card-body">
          <div class="container-md">
            <div class="row">
              <!--            <div class="col">-->
              <!--              <img :src="snippet.topLevelComment.snippet.authorProfileImageUrl" alt="Avatar"-->
              <!--                   class="rounded-circle mr-3 align-self-start ">-->
              <!--            </div>-->
              <!--            <div class="col-md-auto align-self-start">-->
              <div class="col align-self-start">
                <div class="row">
                  <a :href="snippet.topLevelComment.snippet.authorChannelUrl">{{
                      snippet.topLevelComment.snippet.authorDisplayName
                    }}</a> · {{ formatDate(snippet.topLevelComment.snippet.updatedAt) }}
                </div>
                <div class="row">
                  <p class="card-text text-left">{{ truncate(snippet.topLevelComment.snippet.textDisplay) }}</p>
<!--                  <div v-if="replies!= null">-->
<!--                    <div class="form-check form-switch">-->
<!--                      <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckDefault"-->
<!--                             v-model="show" @click="show=!show">-->
<!--&lt;!&ndash;                      <label class="form-check-label" for="flexSwitchCheckDefault">Ответы:</label>&ndash;&gt;-->
<!--                    </div>-->
<!--                  </div>-->
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
                <!--              <a :href="'https://www.youtube.com/watch?v=' + snippet.topLevelComment.snippet.videoId">URL</a>-->
                <!-- 16:9 aspect ratio -->
                <div class="embed-responsive embed-responsive-16by9">
                  <iframe class="embed-responsive-item"
                          :src="'https://www.youtube.com/embed/'+ snippet.topLevelComment.snippet.videoId + '?controls=0'"></iframe>
                </div>
                <!--              <button v-if="replies!= null" @click="show = !show" type="button" class="btn btn-secondary" data-toggle="collapse" data-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">Ответы</button>-->
                <!--              <div class="custom-control custom-switch">-->
                <!--                <input type="checkbox" :checked="show" class="custom-control-input" id="customSwitch1" @change="show = !show" >-->
                <!--                <label class="custom-control-label" for="customSwitch1">Ответы</label>-->
                <!--              </div>-->

              </div>
            </div>
          </div>
        </div>
      </div>
    </div>


    <!--    <div class="collapse" id="collapseExample">-->
    <!--      <div class="card card-body">-->
    <!--        <template v-if="show && replies!= null">-->
    <!--          <div class="media mt-3">-->
    <!--          <span v-for="(comments, index) in replies" v-bind:key="index">-->
    <!--            <NestedComments-->
    <!--                v-for="{snippet, index} in comments" v-bind:key="index"-->
    <!--                :snippet="snippet"-->
    <!--            ></NestedComments>-->
    <!--          </span>-->
    <!--          </div>-->
    <!--        </template>-->
    <!--&lt;!&ndash;        Some placeholder content for the collapse component. This panel is hidden by default but revealed when the user activates the relevant trigger.&ndash;&gt;-->
    <!--      </div>-->
    <!--    </div>-->


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
    }
  }
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

/*.row {*/
/*  background: rgba(255, 0, 0, .1)*/
/*}*/

/*.row > div {*/
/*  padding: 10px 15px;*/
/*  background: rgba(86, 61, 124, .15);*/
/*  border: 1px solid rgba(86, 61, 124, .2)*/
/*}*/

/*.column > div {*/
/*  padding: 10px 15px;*/
/*  background: rgba(86, 61, 124, .15);*/
/*  border: 1px solid rgba(86, 61, 124, .2)*/
/*}*/
</style>