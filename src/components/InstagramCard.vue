<template>

      <div class="card text-center">
        <div class="card-header text-left">
          By <strong>{{ info.email }}</strong>
        </div>
        <div class="card-body">
          <img @click="changeImage" class="card-img-top" :src="info.url" alt="Maznusmo s unsplasha i bilo bi lijepo da napisemo cija je slika">
        </div>
        <div class="card-footer text-left">
          <div>{{ timeAgo }}</div>

          <ul>
            <li :key="comment.posted_at" v-for="comment in info.comments">{{ comment.comment }}</li>
          </ul>

          <form @submit.prevent="post" class="form-inline mb-5">
            <div class="form-group">
              <input v-model="postComment" type="text" class="form-control" id="imageUrl" placeholder="Any comment?">
            </div>
            <button type="submit" class="btn btn-primary ml-2">Post</button>
          </form>

        </div>
      </div>

</template>

<script>
import moment from 'moment'
import store from '@/store.js'

export default {
  props: [ "info" ],
  data () {
    return store
  },
  methods: {
    changeImage() {
      this.info.time = 'Fetching...'
      fetch("https://source.unsplash.com/1600x900/?nature,water").then(response => {
        this.info.url = response.url
        this.info.time = 'Done.'
      })
    },

    post () {
      db.collection("posts").doc(this.info.id).collection("comments").add({ email: this.userEmail, comment: this.postComment, posted_at: Date.now() })
    }
  },

  computed: {
    timeAgo () {
      return moment(this.info.posted_at).fromNow()
    }
  }
}
</script>
