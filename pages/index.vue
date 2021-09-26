<template>
  <div>
    <br-page
      v-if="page"
      :configuration="configuration"
      :mapping="mapping"
      :page="page"
    >
      <br-component component="menu"></br-component>
    </br-page>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from 'axios'

import {
  ssrRef,
  ref,
  useAsync,
  onMounted,
  useFetch,
  computed,
  unref,
} from '@nuxtjs/composition-api'
import { Configuration, initialize, Page } from '@bloomreach/spa-sdk'
import Menu from '~/components/BrMenu.vue'

export default Vue.extend({
  // setup(props: any, { root }: any) {
  //   const configuration = {
  //     endpoint: `https://vuestorefront.bloomreach.io/delivery/site/v1/channels/channel4/pages`,
  //     httpClient: axios,
  //     path: root.$route.fullPath,
  //   }

  //   const page = useAsync(() => initialize(configuration));

  //   return {
  //     page,
  //     configuration,
  //     mapping: {
  //       menu: Menu,
  //     },
  //   }
  // },
  async asyncData(context) {
    const configuration = {
      endpoint: `https://vuestorefront.bloomreach.io/delivery/site/v1/channels/channel4/pages`,
      path: context.route.fullPath,
    }
    const page = await initialize({
      ...configuration,
      httpClient: axios,
    })
    return {
      configuration,
      page,
    }
  },
  data: () => ({
    mapping: {
      menu: Menu,
    },
  }),
})
</script>
