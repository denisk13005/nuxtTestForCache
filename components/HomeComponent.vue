<template>
  <div>
    <button>
      <a href="/sessions">to</a>
    </button>
    <div v-for="post of posts.results" :key="post.id">
      <h3>{{ post.title }}</h3>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeComponent',
  data() {
    return {
      posts: [],
    }
  },
  async fetch() {
    console.log('test')
    const now = Date.now()
    const cachedPosts = sessionStorage.getItem('cachedPosts')
    const cachedTime = sessionStorage.getItem('cachedTime')

    // Vérifier si nous avons des données en cache et si elles ont été stockées il y a moins d'une minute
    if (cachedPosts && cachedTime && now - parseInt(cachedTime) < 60000) {
      this.posts = JSON.parse(cachedPosts)
    } else {
      // Si les données sont inexistantes ou trop vieilles, fetch de nouvelles données
      const res = await fetch(
        'https://api.themoviedb.org/3/movie/popular?language=en-US&page=1',
        {
          method: 'GET',
          headers: {
            Authorization:
              'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJiMGU5OGZiM2NhZDI3MTI3NTdmNGZkMTAyMDVlZDk4MiIsInN1YiI6IjYxZmUzYjhmZDA1MWQ5MDBkOWJjNGQyYSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.O4b4AJGaL_DAH5r7Ullr1NH1vqpRRRh9W5NuHK8u0mY',
            Accept: 'application/json',
          },
        }
      )
      const newData = await res.json()
      this.posts = newData

      // Mise à jour du cache
      sessionStorage.setItem('cachedPosts', JSON.stringify(newData))
      sessionStorage.setItem('cachedTime', now.toString())
    }
  },
  activated() {
    // appeller fetch de nouveau si le dernier appel date de plus de 30 secondes
    if (this.$fetchState.timestamp <= Date.now() - 30000) {
      console.log('reload')
      this.$fetch()
    }
  },
  mounted() {
    this.$fetch()
  },
}
</script>

<style lang="scss" scoped>
</style>
eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJiMGU5OGZiM2NhZDI3MTI3NTdmNGZkMTAyMDVlZDk4MiIsInN1YiI6IjYxZmUzYjhmZDA1MWQ5MDBkOWJjNGQyYSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.O4b4AJGaL_DAH5r7Ullr1NH1vqpRRRh9W5NuHK8u0mY'
