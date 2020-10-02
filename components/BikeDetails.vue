<template>
  <div class="container p-2">
    <div class="row mb-3">
      <div class="col-12">
        <div class="d-flex justify-content-between align-items-center">
          <h2 class="h4 mb-0 mr-2">N°: {{ bike['serial_number'] }}</h2>

          <b-badge :variant="order.class" class="h-100">
            {{ order.text }}
          </b-badge>
        </div>
      </div>
    </div>

    <div class="row mb-3">
      <div class="col-6">
        <p class="h5 mb-0 mt-1">Batterie</p>
      </div>

      <div class="col-6 position-relative">
        <b-progress :value="bike['battery_level']" :max="100" :variant="batteryVariant" class="battery" striped animated />

        <div class="font-weight-bold position-absolute h5 battery-level">{{ bike['battery_level'] }}%</div>
      </div>
    </div>

    <div class="row">
      <div class="col-6">
        <p class="h5 m-0">Statut</p>
      </div>

      <div class="col-6">
        <p class="h5 m-0 font-italic"> {{ status }}</p>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      bike: {
        type: Object,
        default: {}
      }
    },
    computed: {
      order() {
        return {
          class: this.bike['in_order'] ? 'danger' : 'success',
          text: this.bike['in_order'] ? 'Occupé' : 'Libre'
        }
      },
      batteryVariant() {
        if (this.bike['battery_level'] < 40) return 'danger'
        if (this.bike['battery_level'] < 75) return 'warning'
        return 'success'
      },
      status() {
        const status = {
          1: 'Gratuit',
          2: 'Réservé',
          3: 'Utilisé'
        }

        return status[this.bike['service_status']]
      }
    }
  }
</script>

<style>
  .battery {
    height: 2rem;
  }
  .battery-level {
    top: 5px;
    left: 20px;
  }
</style>

