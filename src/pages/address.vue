<template>
  <div class="address-body">
    <nav-header></nav-header>
    <div class="content">
       这里是结算页面
    </div>
    <nav-footer></nav-footer>
  </div>
</template>

<script>
import NavHeader from '../components/Header'
import NavFooter from '../components/Footer'
export default {
  name: 'addr',
  props: {},
  components:{
    NavHeader,
    NavFooter,
  }
}
</script>

<style scoped>
.address-body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.content {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
