<template>
  <div class="w-100">
    <v-card height="300">
      <v-card-text class="text-center">
        <v-avatar size="160">
          <v-avatar
            color="primary"
            size="88"
          >
            <span class="display-1 white--text">{{ user.username | initials }}</span>
          </v-avatar>
        </v-avatar>
        <h2 class="my-2 primary--text">{{ user.username }}</h2>
        <div class="text-h6">{{ roleName(user.roleId) }}</div>
      </v-card-text>
    </v-card>
  </div>
</template>
<script>
import { mapGetters } from 'vuex'
export default {
  props: {
    user: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
    }
  },
  computed: {
    ...mapGetters({
      roleName: 'auth/roleName'
    })
  }
}
</script>