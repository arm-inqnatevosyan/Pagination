<template>
  <ul class="pagination">
    <li class="pagination-item">
      <a href="#" :class="isInFirstPage? 'disabled':''" :disabled="isInFirstPage" @click.prevent="onClickFirstPage">First</a>
    </li>
    <li class="pagination-item">
      <a href="#" :class="isInFirstPage? 'disabled':''" :disabled="isInFirstPage" @click.prevent="onClickPreviousPage">«</a>
    </li>
    <li v-for="page in pages" :key="page.id" class="pagination-item">
      <a
        href="#"
        :disabled="page.isDisabled"
        :class="{ active: isPageActive(page.name) }"
        @click.prevent="onClickPage(page.name)"
      >{{ page.name }}</a>
    </li>
    <li class="pagination-item">
      <a href="#" :class="isInLastPage? 'disabled':''" :disabled="isInLastPage" @click.prevent="onClickNextPage">»</a>
    </li>
    <li class="pagination-item">
      <a href="#" :class="isInLastPage? 'disabled':''" :disabled="isInLastPage" @click.prevent="onClickLastPage">Last</a>
    </li>
  </ul>
</template>

<script>
export default {
  name: 'PaginationVue1',
  props: {
    maxVisibleButtons: {
      type: Number,
      required: false,
      default: 3
    },
    totalPages: {
      type: Number,
      required: true
    },
    perPage: {
      type: Number,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    }
  },
  computed: {
    isInFirstPage () {
      return this.currentPage === 1
    },
    isInLastPage () {
      if (this.totalPages === 0) {
        return true
      }
      return this.currentPage === this.totalPages
    },
    startPage () {
      // When on the first page
      if (this.currentPage === 1) {
        return 1
      }
      // When on the last page
      if (this.totalPages < this.maxVisibleButtons) {
        return 1
      }
      if (this.currentPage === this.totalPages) {
        return this.totalPages - this.maxVisibleButtons + 1
      }
      // When in between
      return this.currentPage - 1
    },
    endPage () {
      if (this.totalPages === 0) {
        return 1
      }
      if (this.totalPages < this.maxVisibleButtons) {
        return this.totalPages
      }
      return Math.min(this.startPage + this.maxVisibleButtons - 1, this.totalPages)
    },
    pages () {
      const range = []
      for (let i = this.startPage; i <= this.endPage; i++) {
        range.push({
          name: i,
          isDisabled: i === this.currentPage
        })
      }
      return range
    }
  },
  methods: {
    onClickFirstPage () {
      if (this.isInFirstPage) {
        return false
      }
      this.$emit('pagechanged', 1)
    },
    onClickPreviousPage () {
      if (this.isInFirstPage) {
        return false
      }
      this.$emit('pagechanged', this.currentPage - 1)
    },
    onClickPage (page) {
      this.$emit('pagechanged', page)
    },
    onClickNextPage () {
      if (this.isInLastPage) {
        return false
      }
      this.$emit('pagechanged', this.currentPage + 1)
    },
    onClickLastPage () {
      console.log('onClickLastPage')
      if (this.isInLastPage) {
        return false
      }
      this.$emit('pagechanged', this.totalPages)
    },
    isPageActive (page) {
      return this.currentPage === page
    }
  }
}
</script>

<style>
.pagination {
  list-style-type: none;
  float: right;
  margin: 10px 0;
}
.pagination .pagination-item{
  display: inline-block;
  color: #ddd;
}
.pagination .pagination-item a{
  text-decoration: none;
  margin: 5px;
  color: #2c3e50;
}
.pagination .pagination-item a .active{
  background-color: tomato;
  color: #ffffff !important;
  font-weight: bold;
  padding: 3px 8px;
}
button[disabled], html input[disabled] {
  cursor: default;
  color: lightgray;
}
</style>
