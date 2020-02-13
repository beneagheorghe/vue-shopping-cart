<template>
    <div>
        <b-modal id="bv-modal-example" v-model="show">
            <template v-slot:modal-title>
                <h3>{{ product.name }}</h3>
            </template>
            <b-img :src="product.image" width="400" height="400" class="cursor"></b-img>
            <div class="text-center">
                <h6>{{ product.description }}</h6>
            </div>
            <template v-slot:modal-footer>
                <b-button variant="primary" size="lx" class="float-right" @click="show = false">
                    Close
                </b-button>
            </template>
        </b-modal>
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
    data() {
        return {
            show: true
        };
    },
    watch: {
        show() {
            if (!this.show) {
                this.$emit("pp-close");
            }
        }
    }
};
</script>

<style></style>
