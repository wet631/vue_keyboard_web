<template>
  <div class="dashboard-container">
    <!-- <div class="dashboard-text">name: {{ name }}</div> -->
    <inputKeyBoard></inputKeyBoard>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import inputKeyBoard from '@/components/car_keyboard_web/vnp-keyboard.vue'
export default {
  name: 'Dashboard',
  components:{
    inputKeyBoard
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
