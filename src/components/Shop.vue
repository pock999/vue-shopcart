<template>
    <div>
        {{ msg }}
        <ul>
            <li v-for="item in shopList" :key="item.id">
                {{ item }}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props:[
        "shopList"
    ],
    data () {
        return {
            msg:"SHOP啦"
        }
    }
}
</script>

<style lang="scss">

</style>