<template>
  <v-app v-if="isAppReady">
    <!-- Layout component -->
    <component :is="currentLayout" v-if="isRouterLoaded">
      <transition name="fade" mode="out-in">
        <router-view :key="$route.path"/>
      </transition>
    </component>

    <v-snackbar
      v-model="toast.show"
      :timeout="toast.timeout"
      :color="toast.color"
      top
      right
    >
      <div class="d-flex align-center">
        <v-icon class="mr-2">{{ toast.icon }}</v-icon>
        <span>{{ toast.message }}</span>
      </div>
      <template v-slot:action="{ attrs }">
        <v-btn
          icon
          v-bind="attrs"
          @click="toast.show = false"
        >
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </template>
    </v-snackbar>
  </v-app>
</template>

<script>
import { mapState } from 'vuex'

import config from './configs'

// Layouts
import defaultLayout from './layouts/DefaultLayout'
import simpleLayout from './layouts/SimpleLayout'
import authLayout from './layouts/AuthLayout'
import errorLayout from './layouts/ErrorLayout'

/*
|---------------------------------------------------------------------
| Main Application Component
|---------------------------------------------------------------------
|
| In charge of choosing the layout according to the router metadata
|
*/
export default {
  components: {
    defaultLayout,
    simpleLayout,
    authLayout,
    errorLayout
  },
  computed: {
    ...mapState({
      toast: (state) => state.app.toast,
      isAppReady: (state) => state.auth.isAppReady
    }),
    isRouterLoaded: function() {
      if (this.$route.name !== null) return true

      return false
    },
    currentLayout: function() {
      if (this.$route.meta.layout === 'dashboard') {
        return 'defaultLayout'
      } else {
        return (this.$route.meta.layout || 'default') + 'Layout'
      }
    }
  }
}
</script>

<style>
/**
 * Transition animation between pages
 */
.fade-enter-active,
.fade-leave-active {
  transition-duration: 0.2s;
  transition-property: opacity;
  transition-timing-function: ease;
}

.fade-enter,
.fade-leave-active {
  opacity: 0;
}
.prod-rate-header {
  white-space: nowrap;
}
.link-table table tbody tr {
  cursor: pointer;
}
</style>
