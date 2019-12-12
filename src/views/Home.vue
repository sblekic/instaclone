<template>
  <div class="row">
    <div class="col-8">

      <div v-if="authenticated">
        <form @submit.prevent="post" class="form-inline mb-5">
          <div class="form-group">
            <label for="imageUrl">Image URL</label>
            <input v-model="newUrl" type="text" class="form-control ml-2" id="imageUrl" placeholder="Enter the image URL">
          </div>
          <button type="submit" class="btn btn-primary ml-2">Post image</button>
        </form>

        <InstagramCard :key="card.id" :info="card" v-for="card in filteredCards" />
      </div>

    </div>
    <div class="col-4">
      Trazimo sljedece: {{ searchTerm }}
    </div>
  </div>
</template>

<script>
import InstagramCard from '@/components/InstagramCard.vue'
import store from '@/store.js'

export default {
  data () {
    return store;
  },
  computed: {
    filteredCards () {
      // let filtered = []
      // for(let card of this.cards) {
      //   if (card.title.includes(this.searchTerm)) {
      //     filtered.push(card)
      //   }
      // }
      // return filtered;
      return this.cards.filter(card => card.title.includes(this.searchTerm));
    }
  },
  name: 'home',
  components: {
    InstagramCard
  },
  methods: {
    post () {
      db.collection("posts").add({ url: this.newUrl, email: this.userEmail, posted_at: Date.now() })
      this.newUrl = ''
    }
  }
}
</script>

<style lang="scss">

.card-body {
  padding: 0px;
}

img:hover {
  cursor: pointer;
}
</style>