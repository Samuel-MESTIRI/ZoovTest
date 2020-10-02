<template>
  <b-modal id="add-bike" title="Ajouter un vélo" centered hide-footer>
    <b-form @submit.prevent="postBike">
      <b-form-group label="Numéro de série" label-for="serial-number" class="mb-3">
        <b-form-input id="serial-number" v-model="serialNumber" required />
      </b-form-group>

      <div class="row">
        <div class="col-6">
          <b-form-group label="longitude" label-for="longitude" class="mb-3">
            <b-form-input id="longitude" type="number" step="0.0000001" v-model="coordinates.longitude" required />
          </b-form-group>
        </div>

        <div class="col-6">
          <b-form-group label="lattitude" label-for="lattitude" class="mb-3">
            <b-form-input id="lattitude" type="number" step="0.0000001" v-model="coordinates.lattitude" required />
          </b-form-group>
        </div>
      </div>

      <b-form-group label="Statut" label-for="status" class="mb-4">
        <b-form-select id="status" v-model="selectedStatus" :options="statusOptions" required />
      </b-form-group>

      <b-form-checkbox
        id="in-order"
        v-model="inOrder"
        name="in-order"
        class="mb-3"
      >
        Disponible
      </b-form-checkbox>

      <b-overlay :show="isLoading" opacity="0.6" spinner-small spinner-variant="primary" class="d-inline-block float-right">
        <b-button type="submit" variant="primary">Ajouter</b-button>
      </b-overlay>

    </b-form>
  </b-modal>
</template>

<script>
  export default {
    data() {
      return {
        isLoading: false,
        serialNumber: '',
        coordinates: {
          longitude: '',
          lattitude: ''
        },
        inOrder: false,
        selectedStatus: null,
        statusOptions: [
          { value: null, text: 'Choisir une option' },
          { value: 1, text: 'gratuit' },
          { value: 2, text: 'réservé' },
          { value: 3, text: 'utilisé' },
        ]
      }
    },
    methods: {
      async postBike() {
        this.isLoading = true
        const newBike = {
          serial_number: this.serialNumber,
          location: {
            type: "Point",
            coordinates: [this.coordinates.longitude, this.coordinates.lattitude],
          },
          in_order: !this.inOrder,
          service_status: this.selectedStatus,
          battery_level: 100,
        }

        try {
          const response = await this.$axios.$post('https://jsonbox.io/box_a3a39c3848366d681adf', newBike)

          this.$emit('newBike', response)
        } catch (e) {
          console.log(e)
        }

        this.isLoading = false
        this.$bvModal.hide('add-bike')
      }
    }
  }
</script>

