<template>
  <div class="body">
    <div class="left">
      <img src="../assets/logo.png" width="50px"/>
      <span style="color:white;margin-left:10px;">购物车</span>
    </div>
    <div class="right">
    <span style="color:white;margin-right:10px;">浪险</span>
    <a href="https://wavedanger.top/">
      <img src="../assets/me.png" class="img"/>
    </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'v-header',
  props: {
    msg: String
  }
}
</script>

<style scoped>
  .body{
    width:100%;
    height:80px; 
    background-color:#333;
    display:flex;
    justify-content:space-between;
    align-items:center;
  }
  .img{
    width:50px;
    height:50px;
    display:inline-block;
    border-radius:50%;
  }
  .left,.right{
    display:flex;
    align-items:center;
    margin:auto 20px;
  }
</style>
