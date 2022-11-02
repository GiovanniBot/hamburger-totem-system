<template>
  <Navbar :logo="logo_src" :alt="app_name" />
  <router-view/>
  <Footer />
</template>

<script>
import Navbar from './components/Navbar.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'HomeView',
  components: { Navbar, Footer, },
  
  data() {
    return {
      logo_src: '/img/logo-2.png',
      app_name: 'Make Your Burger',
    }
  },
}
</script>

<style>
  body {
    background-color: #fff;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

  .main-container {
    margin: 50px;
    min-height: 250px;
    height: 100vh;
  }

  h1 {
    text-align: center;
    font-size: 42px;
    margin-bottom: 30;
  }
</style>
