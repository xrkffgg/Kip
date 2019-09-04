<template>
  <div id="app">
    <k-ip/>
  </div>
</template>

<script>
import kip from './components/kip'
export default {
  name: 'app',
  components: {
    'k-ip':kip,
  },
}
</script>