<template>
    <div>
        {{ msg }}
        <ul>
            <li v-for="item in cartList" :key="item.id">
                {{ item }}
            </li>
        </ul>
        總計:{{total}}
    </div>
</template>

<script>
export default {
    props:[
        "cartList"
    ],
    data () {
        return {
            msg:"CART啦",
        }
    },
    computed: {
        total:function(){
            //計算購物車總和
            let sum = 0;
            this.cartList.forEach(ele => {
                sum += (ele.amount * ele.price);
            });
            return sum;
        }
    }
}
</script>

<style lang="scss">

</style>