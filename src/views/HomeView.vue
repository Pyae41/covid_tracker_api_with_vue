<template>
  <main v-if="!loading">
      <DataTitle
      :text="title"
      :dataDate="dataDate"/>

      <DataBoxes
      :stats="status"/>

      <CountrySelect
      @get-country="getCountryData"
      :countries="countries"/>

      <button 
      @click ="clearCountryData"
      v-if="status.Country"
      class="bg-green-700 
      text-white rounded 
      p-3 mt-10 
      focus:outline-none hover:bg-green-600">
        Clear Country
      </button>

  </main>
  <main class="flex flex-col align-center justify-center " v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
        Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt=""/>
  </main>
</template>

<script>

import DataTitle from '@/components/DataTitle.vue';
import DataBoxes from '@/components/DataBoxes.vue';
import CountrySelect from '@/components/CountrySelect.vue';

export default {
  name: 'HomeView',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
   data(){
        return{
            loading: true,
            title: 'Global',
            dataDate: '',
            status: {},
            countries: [],
            loadingImage: require('../assets/loading.jpg')
        }
    },
    methods:{
        async fetchCovieData(){
            const response = await fetch('https://api.covid19api.com/summary');
            const data = await response.json();

            return data;
        },
        getCountryData(country){
          this.status = country;
          this.title = country.Country;

        },
        async clearCountryData(){
          this.loading = true;
          const data = await this.fetchCovieData();
          this.title = 'Global';
          this.status = data.Global;
          this.loading = false;
        }
    },
    async created(){
        const data = await this.fetchCovieData();
        
        this.dataDate = data.Date;
        this.status = data.Global;
        this.countries = data.Countries;
        this.loading = false;

    }
}
</script>
