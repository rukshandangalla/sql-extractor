<template>
  <div id="app">
    <title-bar></title-bar>
    <landing-page></landing-page>
  </div>
</template>

<script>
  import LandingPage from '@/components/LandingPage'
  import TitleBar from '@/components/TitleBar'

  export default {
    name: 'eclipse-script-generator',
    components: {
      LandingPage,
      TitleBar
    }
  }
</script>

<style>

</style>
