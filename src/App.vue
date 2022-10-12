<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  created() {
    this.$router.beforeEach((to,from,next)=>{
      if(to=='/login'){
        next()
      }else{
        if (!sessionStorage.getItem('token')) {
          return this.$router.replace('/login')
        }
      }
    })
    /* 每次刷新回到首页 */
   /*  if (this.$router.currentRoute.path != '/login') {
      if(!sessionStorage.getItem('token')){
        return this.$router.replace('/login')
      }
      this.$router.replace('/home')
    } */
   
  },
  computed: {},
};
</script>

<style>

</style>