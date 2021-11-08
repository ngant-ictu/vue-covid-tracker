<template>
  <main v-if='!loading'>
    <DataTitle :title="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats"/>
    <CountrySelect @get-country="countryDataCovid" :countries="countries" />
    <button 
    v-if="stats.Country"
    @click="clearCountryData"
    class="bg-green-700 text-white text-1xl rounded px-5 py-2 mt-4 hover:out-line hover:bg-green-600">Clear Country</button>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-5">Fetching data</div>
    <img :src="loadingImage" class="h-24 w-24 m-auto" alt="">
  </main>
  
</template>

<script>
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes' 
import CountrySelect from '@/components/CountrySelect'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/hourglass.gif')
    }
  },
  methods:{
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data;
    },
    countryDataCovid(country){
      this.stats = country
      this.title = country.Country
      this.loading = false
    },
    async clearCountryData() {
      this.loading = true
      const data = await this.fetchCovidData();
      this.title = 'Global';
      this.stats = data.Global
      this.loading = false
    }
  },
  async created() {
    const data = await this.fetchCovidData();
    console.log(data);
    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  }
}
</script>
