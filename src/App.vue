<script>

import navComponent from '@/components/layouts/Navbar'

export default {
  components: {
    navComponent
  },
  name: "App",
  props: ['nav'],

  computed: {
    appRoutes() {
      return window.appRoutes;
    }
  },
  methods: {},
  mounted() {

  },

  data() {
    return {
    }
  }
}
</script>

<template>
  <div class="main">
    <div class="navigation-wrapper">
      <navComponent/>
    </div>
    <div class="view-wrapper">
       <router-view/>
    </div>

    <div class="container-modals">
    </div>
  </div>
</template>

<style lang="scss">
@import "styles/import_main";

</style>
