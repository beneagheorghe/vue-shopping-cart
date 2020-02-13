<template>
    <div>
        <b-card-group class="center">
            <shop-cart v-for="(product, inx) of shopData.products" :key="inx" :product="product" @addToCard="addToCard" />
        </b-card-group>
    </div>
</template>

<script>
import ShopCart from "./ShopCart";
export default {
    props: ["shopData"],
    components: {
        "shop-cart": ShopCart
    },
    methods: {
        addToCard(item) {
            this.$emit("addToCard", item);
        }
    }
};
</script>

<style scoped>
.center {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-flow: row wrap;
}
</style>
