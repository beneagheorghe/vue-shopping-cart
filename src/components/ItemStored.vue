<template>
    <div>
        <b-card no-body class="overflow-hidden" style="max-width: 400px;">
            <b-row no-gutters>
                <b-col md="4">
                    <b-card-img :src="product.image" class="rounded-0" width="150" height="150"></b-card-img>
                </b-col>
                <b-col md="8">
                    <b-card-body :title="product.name">
                        <p style="margin-top: 50px;">
                            <strong>${{ product.price }}</strong>
                        </p>
                        <b-button size="sm" class="float-right" @click="removeItem(product)">
                            <b-img src="https://www.searchpng.com/wp-content/uploads/2019/03/Hold-butt-715x715.png" width="25" />
                        </b-button>
                    </b-card-body>
                </b-col>
            </b-row>
        </b-card>
    </div>
</template>

<script>
export default {
    props: {
        product: {
            type: Object,
            required: true
        }
    },
    methods: {
        removeItem(product) {
            this.$emit("removeItem", product);
        }
    }
};
</script>

<style></style>
