<template>
    <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :items="3" :dots="false" :nav="false" :autoplay="true">

                        <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.id">
                            <div class="pi-pic">
                                <img :src="itemProduct.galleries[0].photo" alt />
                                <!-- <img src="img/mickey1.jpg" alt /> -->
                                <ul>
                                    <li @click="saveCart(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.galleries[0].photo )" class="w-icon active">
                                        <!-- <router-link to="/cart"> -->
                                        <a href="#"><i class="icon_bag_alt"></i></a> 
                                        <!-- </router-link> -->
                                    </li>
                                    <li class="quick-view">
                                        <router-link :to="'/product/'+itemProduct.id">+ Quick view</router-link>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ itemProduct.type }}</div>
                                <router-link :to="'/product/'+itemProduct.id">
                                <a href="#">
                                    <h5>{{ itemProduct.name }}</h5>
                                </a>
                                </router-link>
                                <div class="product-price">
                                    {{ itemProduct.price }}
                                    <span>$35.00</span>
                                </div>
                            </div>
                        </div>

                    </carousel>
                </div>

                <div class="col-lg-12" v-else>
                    <p>
                         Produk terbaru belum tersedia untuk saat ini.
                    </p>
                </div>
            </div>
        </div>
    </section>
    <!-- Women Banner Section End -->
</template>

<script>
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
    name: "WomenShayna",
    components: {
        carousel
    },
    data() {
        return {
            products: [],
            cartUser: []
        };
    },
    mounted() {
        axios
        .get("http://shayna-backend.belajarkoding.com/api/products")
        .then(res => (this.products = res.data.data.data))
        // eslint-disable-next-line no-console
        .catch(err => console.log(err));

        if (localStorage.getItem("cartUser")) {
                try {
                    this.cartUser = JSON.parse(localStorage.getItem("cartUser"));
                } catch (e) {
                    localStorage.removeItem("cartUser");
                }
        }
    },
     methods: {
        saveCart(idProduct, nameProduct, priceProduct, photoProduct) {
        
        var productStored = {
            "id": idProduct,
            "name": nameProduct,
            "price": priceProduct,
            "photo": photoProduct
        };
        this.cartUser.push(productStored);
        const parsed = JSON.stringify(this.cartUser);
        localStorage.setItem('cartUser', parsed);

        window.location.reload();
        }
    }
};
</script>

<style scoped>
.product-item {
    margin-right: 25px;
}
</style>