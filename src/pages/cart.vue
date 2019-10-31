<template>
  <div class="cart-body">
    <nav-header></nav-header>
  <div class="main">
    <div class="car-title">
        <div>商品信息</div>
        <div>商品金额</div>
        <div>商品数量</div>
        <div>总金额</div>
        <div>编辑</div>
    </div>
    <ul class="car-list">
      <li class="car" v-for="item in cartList" :key="item.productId">
        <div class="car-message">
           <img v-if="!item.checked" src="../assets/checkbox_disabled.png" width="20" height="20"/>
            <img v-if="item.checked" src="../assets/checkbox.png" width="20" height="20"/>
            <img :src="'/imgs/'+item.productImage" width="40" height="40"/>
            <div>{{item.productName}}</div>
        </div>
        <div class="car-price">{{item.productPrice}}</div>
        <div class="car-num">
          <div style="width:20px;height:20px;background-color:#ddd;">-</div>
          <div style="margin:0px 5px;">{{item.productNum}}</div>
          <div style="width:20px;height:20px;background-color:#ddd;">+</div>
        </div>
        <div class="car-price-total">{{item.productNum*item.productPrice}}</div>
        <div class="car-delete">
          <img src="../assets/delete.png" width="20" height="20"/>
        </div>
      </li>
    </ul>
    <div class="car-pay">
        <div class="car-bottom-left">
      <div class="car-all">
        <img src="../assets/checkbox_disabled.png" width="20" height="20"/>
        <!--<img src="../assets/checkbox.png" width="20" height="20"/>-->
        <div>全选</div>
      </div>
      <div class="car-total">
        <span>总价：</span>
        <span>元</span>
      </div>
    </div>
    <div class="car-bottom-right">
        结算
    </div>
    </div>
  </div>
  <Modal></Modal>
  <nav-footer></nav-footer>
  </div>
</template>

<script>
import NavHeader from '../components/Header'
import NavFooter from '../components/Footer'
import Modal from '../components/Modal'
export default {
  name: 'cart',
  data(){
    return{
      cartList:[]
    }
  },
  components:{
    NavHeader,
    NavFooter,
    Modal
  },
  mounted(){
    this.init();
  },
  methods:{
    init(){
      this.axios.get("/mock/cart.json").then((response)=>{
       let res=response;
       this.cartList=res.data.data;
       window.console.log(this.cartList)
      })
    }
  }
}
</script>

<style scoped>
.cart-body{
  display:flex;
  flex-direction:column;
  min-height:100vh;
}
.main{
  width:100%;
  background-color:#eee;
  padding:20px 0px;
  flex:1;
}
.car-title{
  height:30px;
  line-height:30px;
  margin:0px 5px;
  background-color:#333;
  display:flex;
  color:white;
}
.car-title>div:nth-child(1){
  flex:3;
}
.car-title>div:nth-child(2){
  flex:2;
}
.car-title>div:nth-child(n+3){
  flex:1;
}
.car-list{
  margin:0;
  padding:0;
}
.car{
  display:flex;
  background-color:white;
  margin:0px 5px 0px;
  height:90px;
  justify-content:center;
  align-items:center;
  border-bottom:1px solid #ddd;
}
.car-message{
  flex:3;
  display:flex;
  align-items:center;
}
.car-message>img:nth-child(1){
  margin-left:20px;
}
.car-message>img:nth-child(2){
  margin-left:10px;
}
.car-message>div{
  margin-left:10px;
}
.car-price{
  flex:2;
}
.car-num{
  flex:1;
  display:flex;
  align-items:center;
  justify-content:center;
}
.car-price-total{
  flex:1;
}
.car-delete{
  flex:1;
}
.car-pay{
   display:flex;
   background-color:white;
    margin:10px 5px;
}
.car-bottom-left{
  display:flex;
  flex:1;
  justify-content:space-between;
  padding:10px 20px;
}
.car-bottom-right{
  width:80px;
  display:flex;
  justify-content:center;
  align-items:center;
  background-color:#ddd;
}
.car-all,.car-total{
  display:flex;
}
.car-all img{
  margin-right:10px;
}
</style>
