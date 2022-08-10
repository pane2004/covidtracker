<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBox :stats="stats"/>
    <CountrySelect @get-country="getData" :countries="countries"/>
    <button @click="clearCountry" v-if="stats.Country"
    class="bg-green-700 text-white rounded p-3 mt-10 
    focus:outline-none hover:bg-green-500">
      Clear Countries
    </button>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data...
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="" />
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle'
import DataBox from '@/components/DataBox'
import CountrySelect from '@/components/CountrySelect'

export default {
  name: 'HomeView',
  components: {
    DataTitle,
    DataTitle,
    DataBox,
    CountrySelect,
},
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      status: {},
      countries: [],
      loadingImage: require('../assets/load.gif'),
    }
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },
    getData(country) {
      this.stats = country
      this.title = country.Country
    },
    async clearCountry() {
      this.loading = true
      const data = await this.fetchCovidData()
      this.title = 'Global'
      this.stats = data.Global
      this.dataDate = data.Date
      this.loading = false
    }
  },
  async created() {
    const data = await this.fetchCovidData()
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  }
}
</script>
