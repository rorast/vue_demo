<template>
  <div class="m-geo">
    <i class="el-icon-location"/>{{ city }}
    <router-link
      class="changeCity"
      to="/changeCity">切換城市</router-link>
    [高雄 台中 台北]
  </div>
</template>

<script>
export default {
  city:['高雄','台中','台北']
}
</script>

<style lang="css">
</style>
