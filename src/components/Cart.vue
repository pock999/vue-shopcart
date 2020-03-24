<template>
    <div>
        {{ msg }}
        <ul class="cart-list">
            <li v-for="item in cartList" :key="item.goodsID" class="cart-item">
                <div class="cart-img-contain">
                    <img v-bind:src="item.img" v-bind:alt="item.name">
                    <span class="cart-id">{{ item.goodsID }}</span>
                </div>
                <div class="cart-info">
                    <div class="cart-name">品名:{{ item.name }}</div>
                    <div class="cart-price">{{item.price}}元</div>
                    <div class="cart-amount">購物車數量:{{ item.amount }}</div>
                    <button class="add-amount" @click="addAmount(item)">&nbsp;＋&nbsp;</button>
                    <button class="minus-amount" @click="minusAmount(item)">&nbsp;－&nbsp;</button>
                </div>
            </li>
        </ul>
        <div id="tail">
            <div id="total">總計:${{ total }}</div>
            <button id="payment" @click="payment()">結帳</button>
            <div></div>
        </div>
    </div>
</template>

<script>
export default {
    props:[
        "cartList","shopList"
    ],
    data () {
        return {
            msg:"購物車數量一增加，剩餘數量就會減少",
        }
    },
    computed: {
        total:function(){
            //計算購物車總和
            let sum = 0;
            this.cartList.forEach(ele => {
                sum += (ele.amount * ele.price);
            });
            return sum.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
        },
    },
    methods:{
        /** 更新購物車以及購物清單 **/
        updateList:function(newCartList,newProductList){
            this.$emit('updateList',newCartList,newProductList);
        },
        /** 增加數量 **/
        addAmount:function(item){
            const newCartList = this.cartList.slice(0);
            const newProductList = this.shopList.slice(0);
            newCartList.forEach(ele => {
                if(ele.goodsID == item.goodsID){
                    ele.amount += 1;
                }
            });
            newProductList.forEach(ele => {
                if(ele.id == item.goodsID){
                    ele.amount -= 1;
                }
            });
            this.updateList(newCartList,newProductList);
        },
        /** 減少數量 **/
        minusAmount:function(item){
            const newCartList = this.cartList.slice(0);
            const newProductList = this.shopList.slice(0);
            newCartList.forEach((ele, index, object) => {
                if(ele.goodsID == item.goodsID){
                    ele.amount -= 1;
                    if(ele.amount == 0){
                        object.splice(index, 1);
                    }
                }
            });
            newProductList.forEach(ele => {
                if(ele.id == item.goodsID){
                    ele.remainingAmount += 1;
                }
            });
            this.updateList(newCartList,newProductList);
        },
        /** 結帳 **/
        payment:function(){
            alert("一共" + this.total + "元");
            const newCartList = [];
            const newProductList = this.shopList.slice(0);
            this.updateList(newCartList,newProductList);
        }
    }
}
</script>

<style lang="scss">
    @import "./../scss/Gloabal.scss";
    .cart-list{
        max-width:960px;
        margin: 5px auto;
        .cart-item{
            position: relative;
            display: grid;
            grid-template-columns: 1fr 2fr;
            height:200px;
            margin:10px auto;
            &:hover{
                background-color: $hover-gray;
            }
            .cart-img-contain{
                position: relative;
                top:15px;
                left:70px;
                width:200px;
                height:170px;
                img{
                    width:100%;
                    height:100%;
                }
                .cart-id{
                    display: block;
                    z-index: 1;
                    position: relative;
                    background-color: $white;
                    line-height:30px;
                    width:30px;
                    height:30px;
                    bottom:175px;
                    left:0;
                    font-weight: bold;
                }
            }
            .cart-info{
                position: relative;

                .cart-name{
                    position: relative;
                    font-size: 30px;
                    font-weight: bolder;
                    left:-200px;
                    top:70px;
                }
                .cart-price{
                    font-size: 24px;
                    font-weight: bolder;
                }
                .cart-amount{
                    color:red;
                    font-weight: bolder;
                }
                .add-amount,.minus-amount{
                    font-size: 30px;
                    line-height: 28px;
                    background-color: $light-black;
                    color:$white;
                    border:5px solid $light-black;
                    border-radius: 7px;
                    margin:5px 10px;
                    &:hover{
                        cursor: pointer;
                        color:$light-black;
                        background-color: $white;
                    }
                }
            }
        }
    }
    #tail{
        display: grid;
        grid-template-columns: 4fr 1fr 1fr;
        max-width:900px;
        margin: 5px auto;
        font-size:28px;
        #total{
            font-weight: bold;
        }
        #payment{
            font-size: 22px;
            line-height: 28px;
            background-color: $light-black;
            color:$white;
            border:5px solid $light-black;
            border-radius: 5px;
            margin:5px 10px;
            font-weight: 500;
            &:hover{
                cursor: pointer;
                color:$light-black;
                background-color: $white;
            }
        }
    }
    @media screen and (max-width: $bigpad-screen){
        .cart-list{
            .cart-item{
                .cart-info{
                    .cart-name{
                        left:-130px;
                        font-size: 22px;
                    }
                }
            }
        }
    }
    @media screen and (max-width: $pad-screen) {
        .cart-list{
            .cart-item{
                .cart-info{
                    .cart-name{
                        left:0px;
                        top:150px;
                        font-size: 22px;
                    }
                }
            }
        }
    }
    @media screen and (max-width: $phone-screen) {
        .cart-list{
            .cart-item{
                grid-template-columns: auto;
                height:600px;
                .cart-img-contain{
                    position: static;
                    width:90%;
                    height:auto;
                    margin:0 auto;
                    .cart-id{
                        display: none;
                    }
                }
                .cart-info{
                    .cart-name{
                        position: static;
                        margin-top:50px;
                    }
                }
            }
        }
    }
</style>