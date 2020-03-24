<template>
  <div id="app">
    <nav>
      <div id="web-name">SHOPPING CART</div>
      <ul id="nav-list">
        <li :class="{'this-tab':ShopTab}" @click="chageTab()">商品列表</li>
        <li :class="{'this-tab':CartTab}" @click="chageTab()">購物車</li>
      </ul>
    </nav>
    <div v-if="ShopTab">
      <Shop :shop-list="GoodsList"
      ></Shop>
    </div>
    <div v-if="CartTab">
      <Cart :cart-list="CartList"
      ></Cart>
    </div>
  </div>
</template>

<script>
import Shop from './components/Shop.vue'
import Cart from './components/Cart.vue'
export default {
  name: 'App',
  components: {
    Shop,
    Cart
  },
  data () {
    return {
      ShopTab: true,
      CartTab: false,
      /** 表示目前分頁狀態，預設是顯示購物頁 **/
      /** remainingAmount表示剩餘數量 **/
      GoodsList:[
        {id:1,name:"商品1", price:300, remainingAmount:5, img:"https://fakeimg.pl/350x200/?text=Product1"},
        {id:2,name:"商品2", price:720, remainingAmount:2, img:"https://fakeimg.pl/350x200/?text=Product2"},
        {id:3,name:"商品3", price:599, remainingAmount:15, img:"https://fakeimg.pl/350x200/?text=Product3"},
        {id:4,name:"商品4", price:199, remainingAmount:0, img:"https://fakeimg.pl/350x200/?text=Product4"},
        {id:5,name:"商品5", price:1500, remainingAmount:3, img:"https://fakeimg.pl/350x200/?text=Product5"},
      ],
      /** amount表示目前購物車之商品數量 **/
      CartList:[
        {goodsID:2,name:"商品2", price:720, amount:7, img:"https://fakeimg.pl/350x200/?text=Goods2"},
        {goodsID:4,name:"商品4", price:199, amount:1, img:"https://fakeimg.pl/350x200/?text=Goods4"},
      ]
    }
  },
  methods:{
    chageTab:function(){
      this.ShopTab = !this.ShopTab;
      this.CartTab = !this.CartTab;
    }
  }
}
</script>

<style lang="scss">

@import "./scss/App.scss" ;
*{
  margin:0;
  padding:0;
  font-family: $font-style;
}
li{
  list-style-type:none;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
