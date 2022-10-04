<template>

  <div class="login-container">

      <app-main />


    <div class="footer" v-html="siteData.copyRight" />

  </div>

</template>

<script>
import { mapGetters } from 'vuex'
import AppMain from '@/layout/components/AppMain'

export default {
  name: 'LoginLayout',
  components: { AppMain },
  computed: {
    ...mapGetters([
      'siteData'
    ])
  }
}
</script>

<style lang="scss" scoped>
@import "~@/styles/login.scss";

</style>

