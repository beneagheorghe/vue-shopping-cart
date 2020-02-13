<template>
    <div>
        <b-navbar toggleable="lg" type="dark" variant="info">
            <b-navbar-brand href="#">Shopping Cart</b-navbar-brand>
            <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
            <b-collapse id="nav-collapse" is-nav>
                <b-navbar-nav class="ml-auto">
                    <b-nav-form>
                        <!-- @submit.prevent="onSubmit" -->
                        <!-- <b-form-input size="sm" class="mr-sm-2" placeholder="Search" v-model="query"></b-form-input> -->
                        <!-- <b-button size="sm" class="my-2 my-sm-0 mr-5" type="submit">Search</b-button> -->
                        <b-button size="sm" class="my-2 my-sm-0 mr-5" v-b-modal.modal-scrollable>
                            <span style="margin-right: 10px;"
                                ><strong>Items {{ shopData.count }}</strong></span
                            >
                            <b-img height="25" src="http://pngimg.com/uploads/shopping_cart/shopping_cart_PNG38.png" />
                            <span
                                ><strong>$ {{ shopData.totalPrice }}</strong></span
                            >
                        </b-button>
                        <b-modal id="modal-scrollable" scrollable title="Shopping cart" size="lg" v-model="showCart">
                            <b-card-group class="center">
                                <item-stored
                                    v-for="(product, inx) of shopData.storeItem"
                                    :product="product"
                                    :key="inx"
                                    @removeItem="removeItem"
                                    style="margin-bottom: 10px;"
                                />
                            </b-card-group>
                            <div style="text-align: center;">
                                <p v-if="this.shopData.count === 0">Shopping cart is empty :(</p>
                            </div>
                            <template v-slot:modal-footer>
                                <b-button variant="dark" size="lx" class="float-right" @click="$bvModal.hide('modal-scrollable')">
                                    Close
                                </b-button>
                                <b-button variant="info" size="lx" class="float-right" @click="formContacts" v-if="shopData.count > 0">
                                    Order Products
                                </b-button>
                            </template>
                        </b-modal>
                    </b-nav-form>
                </b-navbar-nav>
            </b-collapse>
        </b-navbar>
        <b-button size="sm" class="my-2 ml-5" variant="warning" style="color: white;" @click="filterName">
            Name
            <b-img src="https://cdn4.iconfinder.com/data/icons/ionicons/512/icon-arrow-up-b-512.png" width="25" v-if="filterByName" />
            <b-img src="https://cdn4.iconfinder.com/data/icons/ionicons/512/icon-arrow-down-b-512.png" width="25" v-else />
        </b-button>
        <b-button size="sm" class="my-2 ml-2" variant="success" @click="filterPrice">
            Price
            <b-img src="https://cdn4.iconfinder.com/data/icons/ionicons/512/icon-arrow-up-b-512.png" width="25" v-if="filterByPrice" />
            <b-img src="https://cdn4.iconfinder.com/data/icons/ionicons/512/icon-arrow-down-b-512.png" width="25" v-else />
        </b-button>
        <b-modal id="dialog-thank" scrollable title="Order is Done!" size="lg" v-model="showThank">
            <div style="text-align: center;">
                <p>Thank you for shopping</p>
            </div>
            <template v-slot:modal-footer>
                <b-button variant="dark" size="lx" class="float-right" @click="$bvModal.hide('dialog-thank')">
                    Close
                </b-button>
            </template>
        </b-modal>
        <shop-list :shopData="shopData" @addToCard="addToCard" />
        <left-contacts v-if="show" @pp-close="show = false" @orderIsDone="orderIsDone" />
    </div>
</template>

<script>
import ShopData from "@/ShopData.json";
import ShopList from "./ShopList";
import ItemStored from "./ItemStored";
import LeftContacts from "./LeftContacts";

export default {
    components: {
        "shop-list": ShopList,
        "item-stored": ItemStored,
        "left-contacts": LeftContacts
    },
    data() {
        return {
            show: false,
            showCart: false,
            showThank: false,
            defaultData: ShopData,
            shopData: ShopData,
            query: "",
            store: null,
            selected: null,
            filterByName: false,
            filterByPrice: false,
            options: [
                { value: null, text: "Filter By" },
                { value: "Price", text: "Price" },
                { value: "Name", text: "Name" },
                { value: "c", text: "This is another option" },
                { value: "d", text: "This one is disabled", disabled: true }
            ]
        };
    },
    methods: {
        addToCard(item) {
            this.shopData.count++;
            this.shopData.storeItem.push(item);
            this.shopData.totalPrice += item.price;
            localStorage.setItem("shopData", JSON.stringify(this.shopData));
        },
        // onSubmit() {
        //     this.shopData.products = this.shopData.products.filter(element => element.name.toLowerCase().indexOf(this.query.toLowerCase()) > -1);
        // },
        filterName() {
            let self = this;
            this.filterByName = !this.filterByName;
            this.shopData.products.sort(function(a, b) {
                if (self.filterByName) {
                    if (a.name > b.name) {
                        return 1;
                    }
                    if (a.name < b.name) {
                        return -1;
                    }
                    return 0;
                } else {
                    if (a.name > b.name) {
                        return -1;
                    }
                    if (a.name < b.name) {
                        return 1;
                    }
                    return 0;
                }
            });
        },
        filterPrice() {
            let self = this;
            this.filterByPrice = !this.filterByPrice;
            this.shopData.products.sort(function(a, b) {
                if (self.filterByPrice) {
                    return a.price - b.price;
                } else {
                    return b.price - a.price;
                }
            });
        },
        formContacts() {
            this.show = true;
        },
        orderIsDone(form) {
            this.shopData = this.defaultData;
            this.showCart = false;
            this.showThank = true;
            localStorage.setItem("shopData", JSON.stringify(this.defaultData));
            localStorage.setItem("contact", JSON.stringify(form));
        },
        removeItem(item) {
            this.shopData.storeItem.forEach((element, index) => {
                if (element.id == item.id) {
                    this.shopData.storeItem.splice(index, 1);
                    this.shopData.count--;
                    this.shopData.totalPrice -= element.price;
                }
            });
            this.shopData.products.forEach((element, index) => {
                if (element.id == item.id) {
                    this.shopData.products[index].pick = false;
                }
            });
            localStorage.setItem("shopData", JSON.stringify(this.shopData));
        }
    },
    mounted() {
        if (localStorage.getItem("shopData")) {
            this.shopData = JSON.parse(localStorage.getItem("shopData"));
        }
    }
};
</script>

<style scoped>
span {
    margin-left: 5px;
    color: greenyellow;
}
.center {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-flow: row wrap;
}
</style>
