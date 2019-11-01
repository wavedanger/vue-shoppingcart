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
           <img v-show="!item.checked" src="../assets/checkbox_disabled.png" width="20" height="20" @click="editCart('check',item)"/>
            <img v-show="item.checked" src="../assets/checkbox.png" width="20" height="20" @click="editCart('check',item)"/>
            <img :src="'/imgs/'+item.productImage" width="40" height="40"/>
            <div>{{item.productName}}</div>
        </div>
        <div class="car-price">{{item.productPrice}}</div>
        <div class="car-num">
          <div style="width:20px;height:20px;background-color:#ddd;cursor:pointer;" @click="editCart('minus',item)">-</div>
          <div style="margin:0px 5px;">{{item.productNum}}</div>
          <div style="width:20px;height:20px;background-color:#ddd;cursor:pointer;" @click="editCart('add',item)">+</div>
        </div>
        <div class="car-price-total">{{(item.productNum*item.productPrice)|currency}}</div>
        <div class="car-delete">
          <a href="javascript:;" @click="delCartConfirm(item)">
            <img src="../assets/delete.png" width="20" height="20"/>
          </a>
        </div>
      </li>
    </ul>
    <div class="car-pay">
        <div class="car-bottom-left">
      <div class="car-all" @click="toggleCheckAll()">
        <img src="../assets/checkbox_disabled.png" width="20" height="20" v-show="!checkAllFlag"/>
        <img src="../assets/checkbox.png" width="20" height="20"  v-show="checkAllFlag"/>
        <div>全选</div>
      </div>
      <div class="car-total">
        <span>总价：{{totalPrice|currency}}</span>
        <span></span>
      </div>
    </div>
    <div class="car-bottom-right" :class="{'ablePay':checkedCount}" @click="checkOut()">
        结算
    </div>
    </div>
  </div>
  <nav-footer></nav-footer>
  <modal :mdShow="modalConfirm" @close="closeModal">
    <template v-slot:message>
      <p slot="message">你确认要删除此条数据吗？</p>
    </template>
    <template v-slot:btnGroup>
      <a href="javascript:" @click="closeModal">取消</a>
      <a href="javascript:" @click="delCart">确定</a>
    </template>
  </modal>
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
      modalConfirm:false,//弹框显示属性
      cartList:[],//购物车列表
      delItem:'',//要删除的一项
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
  computed:{
    checkAllFlag(){
      return this.cartList.every((item)=>{
        return item.checked;
      });
    },
    checkedCount(){
      return this.cartList.some((item)=>{
        return item.checked;
      })
    },
    totalPrice(){
      let money=0;
      this.cartList.forEach((val)=>{
        if(val.checked){
          money+=val.productNum*val.productPrice;
        }
      })
      return money
    }
  },
  filters:{
    currency:(value)=>{
      if(!value){
        return 0.00;
      } 
      return '¥'+(value*1).toFixed(2)+'元'
    }
  },
  methods:{
    init(){
      this.axios.get("/mock/cart.json").then((response)=>{
       let res=response;
       this.cartList=res.data.data;
       window.console.log(this.cartList)
      })
    },
    editCart(type,item){
      if(type == "add"){
        item.productNum++;
      }else if(type == "minus"&&item.productNum!=0){
        item.productNum--;
      }else if(type=="check"){
        item.checked=!item.checked;
      }
    },
    delCartConfirm(item){
      this.delItem=item;
      this.modalConfirm=true;
    },
    closeModal(){
      this.modalConfirm=false;
    },
    delCart(){
      let delItem=this.delItem;
      this.cartList.forEach((val,index)=>{
        if(delItem.productId==val.productId){
          this.cartList.splice(index,1);
          this.modalConfirm=false;
        }
      })
    },
    toggleCheckAll(){
      let flag=!this.checkAllFlag;
      this.cartList.forEach((item)=>{
        item.checked=flag;
      })
    },
    checkOut(){
      if(this.checkedCount){
        this.$router.push({
          path:'/address'
        })
      }
    }
  }
}
</script>

<style scoped>
.cart-body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.main {
  width: 100%;
  background-color: #eee;
  padding: 20px 0px;
  flex: 1;
}
.car-title {
  height: 30px;
  line-height: 30px;
  margin: 0px 5px;
  background-color: #333;
  display: flex;
  color: white;
}
.car-title > div:nth-child(1) {
  flex: 3;
}
.car-title > div:nth-child(2) {
  flex: 2;
}
.car-title > div:nth-child(n + 3) {
  flex: 1;
}
.car-list {
  margin: 0;
  padding: 0;
}
.car {
  display: flex;
  background-color: white;
  margin: 0px 5px 0px;
  height: 90px;
  justify-content: center;
  align-items: center;
  border-bottom: 1px solid #ddd;
}
.car-message {
  flex: 3;
  display: flex;
  align-items: center;
}
.car-message > img:nth-child(1) {
  margin-left: 20px;
  cursor: pointer;
}
.car-message > img:nth-child(2) {
  margin-left: 20px;
  cursor: pointer;
}
.car-message > img:nth-child(3) {
  margin-left: 10px;
}
.car-message > div {
  margin-left: 10px;
}
.car-price {
  flex: 2;
}
.car-num {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}
.car-price-total {
  flex: 1;
}
.car-delete {
  flex: 1;
}
.car-pay {
  display: flex;
  background-color: white;
  margin: 10px 5px;
}
.car-bottom-left {
  display: flex;
  flex: 1;
  justify-content: space-between;
  padding: 10px 20px;
}
.car-bottom-right {
  width: 80px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ddd;
}
.car-all,
.car-total {
  display: flex;
}
.car-all img {
  margin-right: 10px;
  cursor: pointer;
}
.ablePay {
  background-color: crimson;
  color: white;
  cursor: pointer;
}
</style>
