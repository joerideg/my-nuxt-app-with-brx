<template>
  <div>
    <br-page
      v-if="page"
      :configuration="configuration"
      :mapping="mapping"
      :page="page"
    >
      <div class="page-body">
        <div class="layout">
          <h1>Top</h1>
          <br-component component="top" />
        </div>
        <div class="layout">
          <h1>Main</h1>
          <br-component component="main" />
        </div>
        <div class="layout">
          <h1>Right</h1>
          <br-component component="right" />
        </div>
        <div class="layout">
          <h1>Bottom</h1>
          <br-component component="bottom" />
        </div>
      </div>
    </br-page>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { initialize } from '@bloomreach/spa-sdk'

import BrMenu from '~/components/BrMenu.vue'
import BrBannerVue from '~/components/BrBanner.vue'

export default Vue.extend({
  data(): any {
    return {
      mapping: {
        menu: BrMenu,
        Banner: BrBannerVue,
      },
      configuration: {
        debug: true,
        endpointQueryParameter: 'endpoint',
        path: this.$route.fullPath,
        httpClient: this.$nuxt.$axios,
      },
      page: undefined,
    }
  },
  async mounted() {
    this.page = await initialize(this.configuration)
  },
})
</script>

<style scoped>
.page-body {
  display: block;
  height: 100%;
  width: 100%;
}

.layout {
  display: block;
  min-height: 50px;
}
</style>
