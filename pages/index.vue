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
  // this setup method wont work, the value of 'page' passed to the BrMenu
  // component is not the same (by reference) as the value returned from the
  // initialize function (it does work when you would set the value on client side)
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
  // Using normal asyncData hooks does work
  // async asyncData(context) {
  //   const configuration = {
  //     endpoint: `https://vuestorefront.bloomreach.io/delivery/site/v1/channels/channel4/pages`,
  //     path: context.route.fullPath,
  //   }

  //   const page = await initialize({
  //     ...configuration,
  //     httpClient: context.$axios,
  //   })

  //   return {
  //     configuration,
  //     page,
  //   }
  // },
  // data(): any {
  //   return {
  //     mapping: {
  //       menu: BrMenu,
  //     },
  //   }
  // },
})
</script>
