<template>
<div class="container-fluid" id="app">
  <vnHeader></vnHeader>
  <br>
  <vnNewsList></vnNewsList>
  <vnFooter></vnFooter>
</div>
</template>
</div>

<script>
  import Footer from './components/Footer.vue'
  import NewsList from './components/News-list.vue'
  import Header from './components/Header.vue'

  export default {
    components: {
      vnFooter: Footer,
      vnNewsList: NewsList,
      vnHeader: Header
    },
    data() {
      return {
      }
    }
  }

</script>

<style>
</style>
