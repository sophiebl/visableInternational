<template>
  <div>
    <VCard class="mb-4">
      <VSystemBar />
      <VBanner single-line>
        Filter the restaurants by ratings
        <template #actions>
          <VSelect
            :items="ratingFilterItems"
            filled
            label="choose your rating"
          />
        </template>
      </VBanner>
    </VCard>

    <v-container class="grey lighten-5">
      <v-row no-gutters>
        <v-col
          v-for="company of companies"
          :key="company.id"
          sm="12"
          md="4"
          class="pa-2"
        >
          <VaCompanyCard :company="company" class="mb-6" />
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import VaCompanyCard from '~/components/company-card.vue'

export default {
  name: `page-home`,
  components: {
    VaCompanyCard,
  },
  data() {
    return {
      companies: [],
      minimumRating: 0,
    }
  },
  async fetch() {
    try {
      const companies = await this.$axios.$get(`/companies`)
      console.log('companies', companies)
      this.companies = companies
    } catch (error) {
      this.$nuxt.error(error)
    }
  },
  computed: {
    ratingFilterItems() {
      return [
        { text: `all restaurants`, value: 0 },
        { text: `5 stars`, value: 5 },
        { text: `4 stars or more`, value: 4 },
        { text: `3 stars or more`, value: 3 },
        { text: `2 stars or more`, value: 2 },
        { text: `1 star or more`, value: 1 },
      ]
    },
  },
}
</script>
