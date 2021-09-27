<template>
  <div>
    <br-page
      v-if="page"
      :configuration="configuration"
      :mapping="mapping"
      :page="page"
    >
      <br-component component="menu" />
    </br-page>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { initialize, Page } from '@bloomreach/spa-sdk'
import {
  useRoute,
  useContext,
  onServerPrefetch,
  ssrRef,
  useFetch,
  useAsync,
  ref,
} from '@nuxtjs/composition-api'

import BrMenu from '~/components/BrMenu.vue'

export default Vue.extend({
  setup() {
    const route = useRoute()
    const context = useContext()

    const configuration = {
      endpoint: `https://vuestorefront.bloomreach.io/delivery/site/v1/channels/channel4/pages`,
      path: route.value.fullPath,
      httpClient: context.$axios,
    }

    const page = ssrRef<Page | undefined>(undefined)
    onServerPrefetch(async () => {
      page.value = await initialize(configuration)
    })

    // const page = ref<Page | undefined>(undefined);
    // useFetch(async () => {
    //   page.value = await initialize(configuration);
    // });

    // const page = useAsync(() => initialize(configuration));

    return {
      mapping: {
        menu: BrMenu,
      },
      configuration,
      page,
    }
  },
})
</script>
