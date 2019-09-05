<template>
  <div id="app">
    <k-ip
      style="width: 276px;"
      v-model="arr"
    />
  </div>
</template>

<script>
import kip from './components/kip'
export default {
  name: 'app',
  components: {
    'k-ip':kip,
  },
  data() {
    return {
      arr: [],
    }
  },
}
</script>