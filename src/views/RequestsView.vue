<template>
  <div class="main-container">
    <h1>Manage requests</h1>
    <Dashboard />
  </div>
</template>

<script>
import Dashboard from '../components/Dashboard.vue'

export default {
  name: 'RequestsView',
  components: { Dashboard, },
}
</script>
