<template>
  <div class="row">
    <div class="col-8">

      <InstagramCard/>
      <InstagramCard/>

    </div>
    <div class="col-4">
      Mi smo sidebar
    </div>
  </div>
</template>

<script>
import InstagramCard from '@/components/InstagramCard.vue'

export default {
  name: 'home',
  components: {
    InstagramCard
  }
}
</script>

<style lang="scss">

.card-body {
  padding: 0px;
}

</style>