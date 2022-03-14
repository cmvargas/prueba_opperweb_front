<template>
  <div class="home">
    <HomeListPost />
  </div>
</template>

<script>
// @ is an alias to /src

import HomeListPost from '@/components/post/HomeList.vue'

export default {
  name: 'HomeView',
  components: {
    HomeListPost
  }
}
</script>
