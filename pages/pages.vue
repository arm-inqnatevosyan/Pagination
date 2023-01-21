<template>
  <div>
    <div
      class="w-full flex items-center justify-center text-3xl text-sky-500 font-Rampart font-semibold tracking-normal"
    >
      <h1>Site Reviews</h1>
    </div>
    <div class="p-5 m-auto mt-4 grid xl:grid-cols-4 gap-4 md:grid-cols-2 sm:grid-cols-1">
      <div v-for="item in listItems" :key="item.id">
        <div
          id="bg"
          class="flex flex-col justify-center items-center shadow-3xl shadow-cyan-500/50  text-black h-72 m-auto text-center bg-gray-50"
        >
          <div class="font-bold">
            <h4>Passenger: {{ item.name }}</h4>
          </div>
          <div class="flex">
            <h4>Airline: {{ item.airline[0] && item.airline[0].name }}</h4>
          </div>
          <div>
            <h4>Country: {{ item.airline[0] && item.airline[0].country }}</h4>
          </div>
        </div>
      </div>
    </div>
    <Pagination v-if="listItems" :total-pages="totalPages" :per-page="recordsPerPage" :current-page="page" @pagechanged="onPageChange" />
  </div>
</template>

<script>
import axios from 'axios'
import Pagination from '../components/PaginationVue1.vue'

export default {
  components: {
    Pagination
  },
  data () {
    return {
      listItems: [],
      page: 1,
      totalPages: 0,
      totalRecords: 0,
      recordsPerPage: 10,
      enterpageno: ''
    }
  },
  created () {
    this.loadListItem()
  },
  methods: {
    loadListItem () {
      axios.get(`https://api.instantwebtools.net/v1/passenger?page=${this.page}&size=${this.recordsPerPage}`)
        .then((response) => {
          console.log('response', response)
          this.listItems = response.data.data
          this.totalPages = Math.floor(response.data.totalPassengers / this.recordsPerPage) // Calculate total records
          this.totalRecords = response.data.totalPassengers
        })
    },
    onPageChange (page) {
      this.page = page
      this.loadListItem()
    }
  }
}

</script>
<style scoped>
#bg {
  background: url("static/img.png");
}
</style>
