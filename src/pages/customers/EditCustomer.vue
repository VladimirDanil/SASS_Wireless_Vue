<template>
  <div class="flex-grow-1">
    <div class="d-flex align-center py-3">
      <div>
        <div class="display-1">Edit Company</div>
        <v-breadcrumbs :items="breadcrumbs" class="pa-0 py-2"></v-breadcrumbs>
      </div>
    </div>

    <v-tabs v-model="tab" :show-arrows="false" background-color="transparent">
      <v-tab to="#tabs-account">Account</v-tab>
      <v-tab to="#tabs-information">Information</v-tab>
    </v-tabs>

    <v-tabs-items v-model="tab">
      <v-tab-item value="tabs-account">
        <account-tab
          :user="user"
          :companies="companies"
        ></account-tab>
      </v-tab-item>

      <v-tab-item value="tabs-information">
        <information-tab :user="user"></information-tab>
      </v-tab-item>
    </v-tabs-items>
  </div>
</template>

<script>
/*
|---------------------------------------------------------------------
| Edit Customer Page Component
| url: /customers/edit/:id
|---------------------------------------------------------------------
|
| Edit certain customer
*/
import { mapState, mapActions } from 'vuex'

import AccountTab from '../../components/customers/edit/AccountTab'
import InformationTab from '../../components/customers/edit/InformationTab'

export default {
  components: {
    AccountTab, InformationTab
  },
  data() {
    return {
      tab: null,

      breadcrumbs: [
        {
          text: 'Companies',
          to: '/customers',
          exact: true
        },
        {
          text: 'Edit Company'
        }
      ]
    }
  },
  computed: {
    ...mapState({
      user: (state) => state.users.user,
      companies: (state) => state.customers.companies
    })
  },
  mounted() {
    this.open()
  },
  methods: {
    ...mapActions({
      getCompanies: 'customers/getCompanies',
      openEditUser: 'users/openEditUser'
    }),
    open() {
      this.getCompanies()
      this.openEditUser(this.$route.params.id)
    }
  }
}
</script>
