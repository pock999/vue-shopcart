<template>
    <div>
        {{ msg }}
        <ul class="product-list">
            <li v-for="item in shopList" :key="item.id" class="product-item">
                <div class="product-img-contain">
                    <img v-bind:src="item.img" v-bind:alt="item.name">
                    <span class="product-id">{{ item.id }}</span>
                </div>
                <div class="product-info">
                    <div class="product-name">品名:{{ item.name }}</div>
                    <div class="product-price">{{item.price}}元</div>
                    <div class="product-remain-amount">剩餘數量:{{ item.remainingAmount }}</div>
                    <button v-if="item.remainingAmount>0" class="add-cart"
                    @click="addToCart(item)">加入購物車</button>
                    <div v-else class="disable" disabled>沒有庫存了</div>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props:[
        "shopList","cartList"
    ],
    data () {
        return {
            msg:"加入購物車即可保留，所以加入購物車剩餘數量就會減少了"
        }
    },
    methods:{
        /** 更新購物車以及購物清單 **/
        updateList:function(newCartList,newProductList){
            this.$emit('updateList',newCartList,newProductList);
        },
        /** 加入至購物車 **/
        addToCart:function(item){
            const newCartList = this.cartList.slice(0);
            const newProductList = this.shopList.slice(0);
            let finded = false;
            newCartList.forEach(ele => {
                if(ele.goodsID == item.id){
                    finded = true;
                    ele.amount += 1;
                }
            });
            if(!finded){
                newCartList.push({goodsID:item.id,name:item.name, price:item.price, amount:1, img:item.img});
            }
            newProductList.forEach(e => {
                if(e.id == item.id){
                    e.remainingAmount -= 1;
                }
            });
            this.updateList(newCartList,newProductList);
        }
    }
}
</script>

<style lang="scss">
    @import "./../scss/Gloabal.scss";
    
    .product-list{
        display: grid;
        max-width:960px;
        margin: 5px auto;
        grid-template-columns: auto auto auto;
        .product-item{
            position: relative;
            display: grid;
            grid-template-columns: auto auto;
            height:200px;
            &:hover{
                background-color: $hover-gray;
            }
            .product-name{
                font-size: 20px;
                font-weight: bolder;
            }
            .product-price{
                font-size: 24px;
                font-weight: bolder;
               
            }
            .product-remain-amount{
                color:red;
                font-weight: bolder;
                
            }
            .add-cart{
                
                font-size: 18px;
                line-height: 22px;
                background-color: $bn-color;
                color:$white;
                border-radius: 2px;
                margin-top:5px;
                padding:3px 5px;
                border: 0;
                &:hover{
                    cursor: pointer;
                    color:$white;
                    background-color: $bn-color-hover;
                }
            }
            .disable{
                font-size: 20px;
                font-weight: bold;
                color:$dark-gray;
            }
            .product-img-contain{
                position: relative;
                top:10px;
                left:20px;
                width:155px;
                height:120px;
                img{
                    width: 100%;
                    height: 100%;
                }
                .product-id{
                    display: block;
                    z-index: 1;
                    position: relative;
                    background-color: $white;
                    line-height:30px;
                    width:30px;
                    height:30px;
                    bottom:125px;
                    left:0;
                    font-weight: bold;
                    
                }
            }
        }
    }
    @media screen and (max-width: $bigpad-screen){
        .product-list{
            grid-template-columns: auto auto;
            
        }
    }
    @media screen and (max-width: $pad-screen) {
        .product-list{
            grid-template-columns: auto;
            
        }
    }
    @media screen and (max-width: $phone-screen) {
        .product-list{
            grid-template-columns: auto;
            .product-item{
                grid-template-columns: auto;
                height:600px;
                .product-img-contain{
                    width:90%;
                    height:auto;
                    .product-id{
                        display: none;
                    }
                }
                .product-info{
                    margin-top:50px;
                }
            }
            
        }
    }
</style>