<template>
  <vuestic-layout v-layout>
    <!--<app-navbar :isOpen="opened" @toggle-menu="toggleSidebar"/>-->
    <app-sidebar :isOpen="opened" @toggle-menu="toggleSidebar"/>
    <main
      slot="content"
      id="content"
      class="content va-layout gutter--lg fluid"
      role="main"
    >
      <vuestic-pre-loader
        v-show="isLoading"
        class="pre-loader"
      />
      <router-view/>
    </main>
    <span slot="footer">
      Need Help? Contact Support<a href="#" target="_blank"> 302-223-3322</a>
    </span>
  </vuestic-layout>
</template>

<script>
import VuesticLayout
  from '../../vuestic-theme/vuestic-components/vuestic-layout/VuesticLayout'
import AppNavbar from './app-navbar/AppNavbar'
import AppSidebar from './app-sidebar/AppSidebar'
import AppBreadcrumbs from './app-breadcrumbs/AppBreadcrumbs'
import Layout from '../../vuestic-theme/vuestic-directives/Layout'
import { mapGetters } from 'vuex'

export default {
  name: 'app-layout',
  components: {
    VuesticLayout,
    AppNavbar,
    AppSidebar,
    AppBreadcrumbs,
  },
  directives: {
    layout: Layout,
  },
  data () {
    return {
      opened: true,
    }
  },
  methods: {
    toggleSidebar (opened) {
      this.opened = opened
    },
  },
  computed: {
    ...mapGetters([
      'isLoading',
    ]),
  },
}
</script>
