<!-- eslint-disable vue/require-prop-types -->
<template>
  <div>
    <div class="offset">
      <nav aria-label="Page navigation example" class="mb-4">
        <ul class="pagination flex items-center m-auto justify-around w-pag bg-blue-500 text-black shadow-3xl shadow-sky-500 rounded-md">
          <li class="page-item">
            <button v-if="page != 1" id="prev" type="button" class="page-link" @click="page--">
              Previous
            </button>
          </li>
          <li class="page-item">
            <button
              v-for="pageNumber in pages.slice(page-1, page+5)"
              :key="pageNumber.id"
              type="button"
              class="page-link border-2 border-white p-2 px-3  border-y-0 border-r-0"
              @click="page = pageNumber"
            >
              {{ pageNumber }}
            </button>
          </li>
          <li class="page-item">
            <button v-if="page < pages.length" id="next" type="button" class="page-link" @click="page++">
              Next
            </button>
          </li>
        </ul>
      </nav>
    </div>
  </div>
</template>
<script>

export default {
  name: 'PaginationVue',
  filters: {
    trimWords (value) {
      return value.split(' ').splice(0, 20).join(' ') + '...'
    }
  },
  data () {
    return {
      posts: [''],
      page: 1,
      perPage: 10,
      pages: [],
      users: []
    }
  },
  computed: {
    displayedPosts () {
      return this.paginate(this.posts)
    }
  },
  watch: {
    posts () {
      this.setPages()
    }
  },
  created () {
    this.getPosts()
  },
  methods: {
    async getPosts () {
      const res = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=60')
      const user = await res.json()
      this.posts = user;
      [user.keys()].map(i => ({
        id: (i + 1),
        name: 'Item ' + (i + 1)
      }))
    },
    setPages () {
      const numberOfPages = Math.ceil(this.posts.length / this.perPage)
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index)
      }
    },
    paginate (posts) {
      const page = this.page
      const perPage = this.perPage
      const from = (page * perPage) - perPage
      const to = (page * perPage)
      return posts.slice(from, to)
    }
  }
}
</script>
<style scoped>
#bg {
  background: url("static/img.png");
}
.page-link:nth-last-child(1){
  border-right:2px solid white;
  border-left: none;
}
.page-link:nth-last-child(2){
  border-right: 2px solid white;
}
#next{
  border:none;
}
#prev{
  border:none;
}
</style>
