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
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }
</style>
