<template >
  <keep-alive>
    <home-component keep-alive />
  </keep-alive>
</template>

<script>
import HomeComponent from '../components/HomeComponent.vue'
export default {
  name: 'IndexPage',
  components: { HomeComponent },
}
</script>
