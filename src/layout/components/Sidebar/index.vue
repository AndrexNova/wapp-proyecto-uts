<template>
  <div :class="{'has-logo':showLogo}">
    <logo v-if="showLogo" :collapse="isCollapse" />
    <el-scrollbar wrap-class="scrollbar-wrapper">
      <el-menu
        :default-active="activeMenu"
        :collapse="isCollapse"
        :background-color="variables.menuBg"
        :text-color="variables.menuText"
        :unique-opened="false"
        :active-text-color="variables.menuActiveText"
        :collapse-transition="false"
        mode="vertical"
      >
        <template
          v-for="route in routes"
        >
          <sidebar-item
            v-if="validNav(route)"
            :key="route.path"
            :item="route"
            :base-path="route.path"
          />
        </template>
      </el-menu>
    </el-scrollbar>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Logo from './Logo'
import SidebarItem from './SidebarItem'
import variables from '@/styles/variables.scss'

export default {
  components: { SidebarItem, Logo },
  computed: {
    ...mapGetters([
      'sidebar',
      'users_roles'
    ]),
    routes() {
      return this.$router.options.routes
    },

    activeMenu() {
      const route = this.$route
      const { meta, path } = route
      // if set path, the sidebar will highlight the path you set
      if (meta.activeMenu) {
        return meta.activeMenu
      }
      return path
    },
    showLogo() {
      return this.$store.state.settings.sidebarLogo
    },
    variables() {
      return variables
    },
    isCollapse() {
      return !this.sidebar.opened
    },
    rout() {
      return this.$router
    }
  },
  methods: {
    validNav(navRoute) {
      const navCode = (navRoute?.meta?.code) ? navRoute.meta.code : ''
      const user_rols = this.users_roles
      const found = user_rols.find(element => element.code === navCode)

      if (typeof found === 'object' || navCode === '') {
        return true
      }

      return false
    }
  }
}
</script>
