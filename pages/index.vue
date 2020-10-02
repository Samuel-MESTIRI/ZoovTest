<template>
  <div>
    <Map :bikes="bikes" />

    <b-button v-b-modal.add-bike variant="primary" class="m-3 position-absolute add-bike" pill>
      + Ajouter un  vélo
    </b-button>

    <AddBikeModal @newBike="updateBikes" />
  </div>
</template>

<script>
  import Map from '~/components/Map.Vue'
  import AddBikeModal from '~/components/AddBikeModal.Vue'

  export default {
    components: { Map, AddBikeModal },
    data() {
      return {
        bikes: []
      }
    },
    async mounted() {
      this.bikes = await this.getBikes()
      console.log(this.bikes)
    },
    methods: {
      async getBikes() {
        try {
          return await this.$axios.$get('https://jsonbox.io/box_079892d6984ad9ea4c99')
        } catch (e) {
          console.log(e)
        }
      },
      updateBikes(bike) {
        this.bikes = [...this.bikes, bike]
      }
    }
  }
</script>

<style scoped>
  .add-bike {
    z-index: 1000;
    top: 0;
    right: 0;
  }
</style>
