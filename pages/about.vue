<template>
  <div>
    <NavBar />
    <div
      class="w-full flex items-center justify-center text-3xl text-sky-500 font-Rampart font-semibold tracking-normal"
    >
      <h1>Site Reviews</h1>
    </div>
    <div id="bg" class="p-5 m-auto mt-4 grid xl:grid-cols-4 gap-4 md:grid-cols-2 sm:grid-cols-1">
      <div v-for="user in displayedPosts" :key="user.id">
        <div
          id="bg"
          class="flex flex-col justify-center items-center shadow-3xl shadow-cyan-500/50  text-black h-72 m-auto text-center bg-gray-50"
        >
          <div class="font-bold">
            {{ user.email }}
          </div>
          <div class="flex">
            {{ user.name }}
          </div>
          <div>
            {{ user.body }}
          </div>
        </div>
      </div>
    </div>
    <PaginationVue />
  </div>
</template>

<script>
import PaginationVue from '../components/PaginationVue.vue'
import NavBar from '@/components/NavBar.vue'

export default {
  name: 'AboutVue',
  components: { NavBar, PaginationVue }
}
</script>
<style scoped>
#bg {
  background: url("static/img.png");
}
</style>
