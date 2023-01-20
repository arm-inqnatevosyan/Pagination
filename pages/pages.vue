<template>
  <div id="app">
    <h2>Vue Pagination</h2>
    <ul class="flex-container">
      <li v-for="item in listItems" :key="item.id" class="flex-item">
        <h4>{{ item.email }}</h4>
        <h4>{{ item.name }}</h4>
        <h4>{{ item.body }}</h4>
      </li>
      <li v-if="listItems.length === 0" class="flex-item center">
        No Record Found
      </li>
    </ul>
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
      axios.get('https://jsonplaceholder.typicode.com/comments?_limit=40')
        .then((response) => {
          console.log('response', response)
          this.listItems = response.data
          this.totalPages = Math.floor(response.data.length / this.recordsPerPage) // Calculate total records
          this.totalRecords = response.data.length
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
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  h2 {
    text-align: center;
  }
  ul li {
    list-style-type: none;
  }
  ul.flex-container {
    padding: 0;
    margin: 0;
    list-style-type: none;
    display: -webkit-box;
    display: -moz-box;
    display: -ms-flexbox;
    display: -webkit-flex;
    display: flex;
    flex-direction: row wrap;
    flex-wrap: wrap;
    justify-content: space-around;
  }
  ul.flex-container .flex-item{
    background: tomato;
      width: calc(100% / 5.5);
      padding: 5px;
      height: auto;
      margin-top: 10px;
      color: white;
      font-weight: bold;
      text-align: center;
  }
  ul.flex-container img{
    display: initial;
    width: 200px;
  }
  .showItems {
    display: inline-block;
    margin-left: -35px;
  }
  .showItems  li {
      list-style-type: none;
      display: inline-block;
      margin-left: 10px;
    }
  </style>
