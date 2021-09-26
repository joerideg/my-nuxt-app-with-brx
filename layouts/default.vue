<template>
  <div>
    <br-page
      v-if="page"
      :configuration="configuration"
      :mapping="mapping"
      :page="page"
    >
      <br-component component="main"></br-component>
      <Nuxt />
    </br-page>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from 'axios'

import {
  onMounted,
  onServerPrefetch,
  ssrRef,
  useRoute,
} from '@nuxtjs/composition-api'
import { initialize, Page } from '@bloomreach/spa-sdk'

export default Vue.extend({
  setup() {
    const environment = 'vuestorefront'
    const channel = 'channel4'

    const route = useRoute()
    const page = ssrRef<Page | undefined>(undefined)

    const configuration = {
      endpoint: `https://${environment}.bloomreach.io/delivery/site/v1/channels/${channel}/pages`,
      httpClient: axios,
      path: route.value.fullPath,
    }

    onServerPrefetch(async () => {
      page.value = await initialize(configuration)
    })

    return {
      page,
      configuration,
      mapping: {},
    }
  },
})
</script>
