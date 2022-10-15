<template>
  <div class="product">
    <HeaderShayna />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12 text-left">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src="image_default" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <carousel :nav="false" :dots="false" class="product-thumbs-track ps-slider">
                                    
                                    <div v-for="ss in productDetails.galleries" :key="ss.id"
                                    class="pt" @click="changeImage(ss.photo)" :class="ss.photo == image_default ? 'active' : '' ">
                                        <img :src="ss.photo" alt="" />
                                    
                                    </div>

                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="productDetails.description"></p>
                                    <h4>${{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <!-- <router-link to="/cart" class="primary-btn pd-cart">Add To Cart</router-link> -->
                                    <router-link to="/cart">
                                    <a @click="saveCart(productDetails.id, productDetails.name, productDetails.price, productDetails.galleries[0].photo)" href="#" class="primary-btn pd-cart">Add To Cart</a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedShayna />
    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from '@/components/HeaderShayna.vue';
import FooterShayna from '@/components/FooterShayna.vue';
import RelatedShayna from '@/components/RelatedShayna.vue';

import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
  name: 'ProductView',
  components: {
    HeaderShayna,
    FooterShayna,
    RelatedShayna,
    carousel
  },
  data() {
    return {
      image_default: '',
      productDetails: [],
      cartUser: []
    };
  },
  methods: {
    changeImage(urlImage) {
        this.image_default = urlImage;
    },
    setDataPicture(data) {
        // replace object productDetails dengan data dari API
        this.productDetails = data;
        // replace value gambar default dengan data dari API (galleries)
        this.image_default = data.galleries[0].photo;
    },
    saveCart(idProduct, nameProduct, priceProduct, photoProduct) {

        var productStored = {
            "id": idProduct,
            "name": nameProduct,
            "price": priceProduct,
            "photo": photoProduct
        }
        this.cartUser.push(productStored);
        const parsed = JSON.stringify(this.cartUser);
        localStorage.setItem('cartUser', parsed);
    }
  },
  mounted() {
        if (localStorage.getItem('cartUser')) {
            try {
                this.cartUser = JSON.parse(localStorage.getItem('cartUser'));
            } catch(e) {
              localStorage.removeItem('cartUser');
              }
        }
        axios
        .get("http://shayna-backend.belajarkoding.com/api/products", {
            params: {
                id: this.$route.params.id
            }
        })
        .then(res => (this.setDataPicture(res.data.data)))
        // eslint-disable-next-line no-console
        .catch(err => console.log(err));
    }
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 14px;
}
</style>
