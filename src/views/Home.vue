<template>
  <div class="row">
    <div class="col-8">
      <div v-if="authenticated">
        <form @submit.prevent="postImage" class="mb-5">
          <croppa :width="400" :height="400" v-model="imageData"></croppa>
          <button type="submit" class="btn btn-primary ml-2">Post image</button>
        </form>

        <InstagramCard :key="card.id" :info="card" v-for="card in filteredCards" />
      </div>
    </div>
    <div class="col-4">Trazimo sljedece: {{ searchTerm }}</div>
  </div>
</template>

<script>
import InstagramCard from "@/components/InstagramCard.vue";
import store from "@/store.js";

export default {
  data() {
    return store;
  },
  computed: {
    filteredCards() {
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
  name: "home",
  components: {
    InstagramCard
  },
  methods: {
    postImage() {
      this.imageData.generateBlob(blobData => {
        if (blobData != null) {
          // ako koristimo "/" u nazivu slike, Storage fino napravi direktorij.
          // Konkretno u ovom primjeru imat ćemo direktorij nazvan po mailu korisnika.
          // Slika će biti nazvana po trenutnom vremenu kako bi imali jedinstveni naziv slike.
          let imageName = this.userEmail + "/" + Date.now() + ".png";

          storage
            .ref(imageName)
            .put(blobData)
            .then(result => {
              result.ref.getDownloadURL()
                .then(url => {
                  db.collection("posts")
                    .add({
                      email: this.userEmail,
                      posted_at: Date.now(),
                      url: url
                    })
                    .then(docRef => {
                      console.log("Document written with ID: ", docRef.id);
                      this.imageData = null;
                    })
                    .catch(e => {
                      console.error("Error adding document: ", error);
                    });
                })
                .catch(e=> {
                  console.error(e)
                })
            })
            .catch(e => {
              console.error(e)
            })
        }
      }); // da... zatvaranje zagrada nakon ovoga noćna je mora!
    }
  }
};
</script>

<style lang="scss">

.card-body {
  padding: 0px;
}

img:hover {
  cursor: pointer;
}

.btn-post {
  width: 200px;
}

.croppa-container {
  border: 3px dashed gray;
  width: 400px;
  display: block;
  margin-bottom: 10px;
}
</style>