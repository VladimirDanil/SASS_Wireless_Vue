<template>
  <div class="d-flex flex-column flex-grow-1">
    <div class="d-flex align-center py-3">
      <div>
        <div class="display-1">Companies</div>
        <v-breadcrumbs :items="breadcrumbs" class="pa-0 py-2"></v-breadcrumbs>
      </div>
<!--       <v-spacer></v-spacer>
      <v-btn v-if="canCreateCompanies" color="primary" to="/customers/add">
        <v-icon left>mdi-plus</v-icon>
        Create Company
      </v-btn> -->
    </div>

    <v-card>
      <v-row dense class="pa-2 align-center" justify="end">
        <v-col sm="12" md="6" class="d-flex text-right align-center">
          <v-text-field
            v-model="searchQuery"
            append-icon="mdi-magnify"
            class="flex-grow-1 mr-md-2"
            solo
            hide-details
            dense
            clearable
            placeholder="e.g. filter for company name, administrator name, etc"
          ></v-text-field>
          <v-btn
            icon
            small
            class="ml-2"
            @click="getCompanyAdmins"
          >
            <v-icon>mdi-refresh</v-icon>
          </v-btn>
        </v-col>
      </v-row>
      <v-data-table
        :headers="headers"
        :items="companyAdmins"
        :search="searchQuery"
        class="flex-grow-1"
        hide-default-footer
        :loading="isTableLoading"
      >
        <template v-slot:item.companyName="{ item }">
          <div>{{ item.company.name }}</div>
        </template>
        <template v-slot:item.administratorName="{ item }">
          <div>{{ item.username }}</div>
        </template>
        <template v-slot:item.createdAt="{ item }">
          <div>{{ item.createdAt | formatDate('yyyy-MM-dd') }}</div>
        </template>
        <template v-slot:item.action="{ item }">
          <div v-if="canCreateCompanies" class="actions">
            <v-btn icon :to="`/customers/edit/${item.id}`">
              <v-icon small>mdi-pencil</v-icon>
            </v-btn>
          </div>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
/*
|---------------------------------------------------------------------
| Customers List Page Component
| url: /customers/list
|---------------------------------------------------------------------
|
| List all customers and customer add/edit options
*/
import { mapState, mapGetters, mapActions } from 'vuex'
export default {
  data() {
    return {
      isLoading: false,
      breadcrumbs: [{
        text: 'Companies',
        disabled: false,
        href: '#'
      }, {
        text: 'List'
      }],

      searchQuery: '',
      headers: [
        { text: 'Company Name', value: 'companyName' },
        { text: 'Administrator Name', value: 'administratorName' },
        { text: 'Created At', value: 'createdAt' },
        { text: '', sortable: false, align: 'right', value: 'action' }
      ]
    }
  },
  computed: {
    ...mapState({
      companyAdmins: (state) => state.customers.companyAdmins,
      isTableLoading: (state) => state.customers.isTableLoading
    }),
    ...mapGetters({
      canCreateCompanies: 'auth/canCreateCompanies'
    })
  },
  mounted() {
    this.open()
  },
  methods: {
    ...mapActions({
      getCompanyAdmins: 'customers/getCompanyAdmins'
    }),
    open() {
      this.getCompanyAdmins()
    }
  }
}
</script>
