<template>
  <div class="home">
    <test msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import test from '@/components/test.vue'

export default {
  name: 'Home',
  components: {
    test
  }
}
</script>
