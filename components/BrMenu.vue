<template>
  <ul v-if="menu" class="navbar-nav col-12" :class="{ 'has-edit-button': page.isPreview() }">
    <br-manage-menu-button :menu="menu" />

    <li v-for="(item, index) in menu.getItems()" :key="index" class="nav-item" :class="{ active: item.isSelected() }">
      <span v-if="!item.getUrl()" class="nav-link text-capitalize disabled">
        {{ item.getName() }}
      </span>

      <a v-else-if="item.getLink().type === TYPE_LINK_EXTERNAL" class="nav-link text-capitalize" :href="item.getUrl()">
        {{ item.getName() }}
      </a>

      <nuxt-link v-else :to="item.getUrl()" class="nav-link text-capitalize">
        {{ item.getName() }}
      </nuxt-link>
    </li>
  </ul>
</template>

<script lang="ts">
import { Component, Menu, Page, TYPE_LINK_EXTERNAL, isMenu } from '@bloomreach/spa-sdk';

import Vue, { PropType } from 'vue';

interface MenuModels {
  menu?: import('@bloomreach/spa-sdk').Reference;
}

export default Vue.extend({
  name: 'BrMenu',
  props: {
    component: {
      type: Object as PropType<Component>,
      default: undefined,
    },
    page: {
      type: Object as PropType<Page>,
      default: undefined,
    },
  },
  data: () => ({ TYPE_LINK_EXTERNAL }),
  computed: {
    menu(): Menu | undefined {
      const menuRef = this.component.getModels<MenuModels>()?.menu;
      const menu = menuRef && this.page.getContent<Menu>(menuRef);

      return isMenu(menu) ? menu : undefined;
    }
  }
})
</script>
