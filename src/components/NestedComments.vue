<template>
        <div class="media-body">
<!--          <div class="card mb-3"> -->
          <div>
            <div class="card-body">
<!--              <div class="col">-->
<!--                <div class="row">-->
<!--                    <div class="col-1.4">-->
<!--                       <img alt="avatar" class="rounded-circle mr-3 align-self-start"-->
<!--                            :src="snippet.authorProfileImageUrl">-->
<!--                    </div>-->
                    <div class="col-xs md-auto align-self-start">
                      <div class="row">
                        <a :href="snippet.authorChannelUrl">
                          {{ snippet.authorDisplayName }}</a> · {{ formatDate(snippet.updatedAt) }}
                      </div>
                      <div class="row">
                        <span class="card-text text-left" v-html="snippet.textDisplay"></span>
<!--                        <p class="card-text text-left">{{ snippet.textDisplay }}</p>-->
                      </div>
                    </div>
<!--                </div>-->
              </div>
<!--            </div>-->
          </div>
        </div>
</template>

<script>
export default {
  name: "NestedComments",
  props: {
    snippet: null
  },

  methods: {
    truncate(v) {
      const newline = v.indexOf('\n')
      return newline > 0 ? v.slice(0, newline) : v
    },

    formatDate(v) {
      return v.replace(/T|Z/g, ' ')
    }
  },
}
</script>

<style scoped>

</style>