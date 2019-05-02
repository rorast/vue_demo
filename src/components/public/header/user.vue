<template>
  <div class="m-user">
    <template v-if="user">
      歡迎您，<span class="username">{{ user }}</span>
      [<router-link to="/exit">退出</router-link>]
    </template>
    <template v-else>
      <router-link
        to="/login"
        class="login">立即登錄</router-link>
      <router-link
        class="register"
        to="/register">注冊</router-link>
    </template>
  </div>
</template>

<script>
export default {
  data(){
    return {
      user:'rorast'
    }
  },
  async mounted(){
    const {status,data:{user}} = await this.$axios.get('/users/getUser')
    if(status===200){
      this.user=user
    }
  }
}
</script>

<style lang="css">

</style>
