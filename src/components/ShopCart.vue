<template>
    <div>
        <b-card :title="product.name" tag="article" style="max-width: 20rem;" class="m-2">
            <b-img @click="showDetail" :src="product.image" width="250" height="250" class="cursor"></b-img>
            <b-card-text>
                {{ product.description | to_description }}
                <span @click="showDetail" class="cursor"><strong>...</strong></span>
            </b-card-text>
            <b-card-text style="text-align: center;">
                <strong>$ {{ product.price }} </strong>
            </b-card-text>
            <b-button variant="outline-primary" v-if="!product.pick" @click="addToCard(product)">Add to Cart</b-button>
            <b-button variant="warning" v-else disabled>Added to Cart</b-button>
        </b-card>
        <detail-cart v-if="show" @pp-close="show = false" :product="product" />
    </div>
</template>

<script>
import DetailCart from "./DetailCart";

export default {
    props: {
        product: {
            type: Object,
            required: true
        }
    },
    components: {
        "detail-cart": DetailCart
    },
    data() {
        return {
            show: false
        };
    },
    filters: {
        to_description(string) {
            return string.trim().slice(0, 70);
        }
    },
    methods: {
        addToCard(product) {
            this.product.pick = true;
            this.$emit("addToCard", product);
        },
        showDetail() {
            this.show = true;
        }
    }
};
</script>

<style scoped>
.cursor:hover {
    color: yellowgreen;
}
.cursor {
    cursor: pointer;
}
</style>
