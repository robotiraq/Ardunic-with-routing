<template>
  <!-- <PageCompenent></PageCompenent> -->

  <div class="fixed w-full z-20 scroll-smooth" ref="wew">
    <div
      v-show="addedtocart"
      class="absolute top-0 w-full flex flex-row bg-green-500 p-8 text-2xl font-semibold text-white items-center justify-center"
    >
      <p class="">Added to Cart</p>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="3"
        stroke="currentColor"
        class="w-7 h-7 ml-2"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M4.5 12.75l6 6 9-13.5"
        />
      </svg>
    </div>
  </div>

  <div class="grid grid-cols-6 bg-slate-50 text-left justify-items-center">
    <div v-if="isFetched" class="max-w-7xl col-span-4 col-start-2 mt-28">
      <!-- <img
          class="rounded-lg max-w-6xl m-auto mb-3"
          src="../src/assets/web.png"
          alt=""
        /> -->

      <!--           v-for="product in paginatedProducts"
   -->
      <input
        class="border px-4 py-2 rounded-lg w-1/3 ml-6"
        type="text"
        v-model="searchQuery"
        placeholder="Search for products..."
      />
      <div class="grid grid-cols-3 mb-2">
        <div
          class="group relative m-4 rounded-3xl bg-white drop-shadow flex flex-col items-center hover:shadow-lg duration-200"
          v-for="product in filteredProducts"
          :key="product._id"
        >
          <router-link :to="{ name: 'product', params: { id: product.slug } }">
            <img
              class="max-h-40 cursor-pointer"
              :src="imagePrefix + product.images[0]"
              alt=""
          /></router-link>
          <div
            class="my-1 bg-stone-800 px-3 py-2 rounded-xl text-lg font-bold text-white flex flex-row items-center justify-center"
          >
            <p>{{ product.price.showPrice }}</p>
            <P class="ml-2">IQD</P>
          </div>
          <div
            v-show="product.stock < 10 && product.stock > 0"
            class="bg-orange-500 text-white px-2 font-semibold py-2 rounded-l-lg bg-opacity-90 absolute top-20 right-0 text-center"
          >
            {{ product.stock }} Pcs Left
          </div>
          <div
            v-show="product.stock == 0"
            class="bg-red-600 text-white px-2 font-semibold py-2 rounded-l-lg bg-opacity-90 absolute top-20 right-0 text-center"
          >
            Out of stock
          </div>
          <button
            class="text-base font-semibold whitespace-pre-line text-stone-800 truncate overflow-hidden px-6 mb-2"
          >
            {{ product.title }}
          </button>
          <div
            @click="setFavorite(product._id)"
            class="absolute top-20 left-0 bg-indigo-400 bg-opacity-50 px-1 rounded-r-lg text-white cursor-pointer"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1"
              stroke="currentColor"
              class="w-10 h-10"
              :class="
                fav.some((e) => e === product._id)
                  ? 'fill-red-400 duration-300 '
                  : ''
              "
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12z"
              />
            </svg>
          </div>
          <div
            @click="addedtoCart()"
            v-show="product.stock > 0"
            class="opacity-0 cursor-pointer absolute bottom-0 font-semibold bg-indigo-400 text-white w-full rounded-b-3xl py-4 group-hover:opacity-100 flex flex-row items-center justify-center transition"
          >
            <p>Add to Cart</p>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-6 h-6 ml-3"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M2.25 3h1.386c.51 0 .955.343 1.087.835l.383 1.437M7.5 14.25a3 3 0 00-3 3h15.75m-12.75-3h11.218c1.121-2.3 2.1-4.684 2.924-7.138a60.114 60.114 0 00-16.536-1.84M7.5 14.25L5.106 5.272M6 20.25a.75.75 0 11-1.5 0 .75.75 0 011.5 0zm12.75 0a.75.75 0 11-1.5 0 .75.75 0 011.5 0z"
              />
            </svg>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  name: "App",
  data() {
    return {
      API: "https://api.ardunic.com/v1/products?s=6",
      isFetched: false,
      products: [],
      imagePrefix: "https://ardunic-images.s3.eu-central-1.amazonaws.com/",
      fav: [],
      addedtocart: false,
      // currentPage: 1,
      perPage: 6,
      currentPage: 1,
      PageCount: 0,
      searchQuery: "",
    };
  },
  created() {
    fetch(this.API)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.products = data.data;
        this.isFetched = true;
      });
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
  },
  computed: {
    filteredProducts() {
      return this.products.filter((product) =>
        product.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
    pageCount() {
      this.PageCount = Math.ceil(this.products.length / this.perPage);
      return Math.ceil(this.products.length / this.perPage);
    },
    paginatedProducts() {
      const startIndex = (this.currentPage - 1) * this.perPage;
      const endIndex = startIndex + this.perPage;
      const productsCopy = [...this.products]; // Make a copy of this.products
      return productsCopy.slice(startIndex, endIndex);
    },
    pages() {
      const pages = [];
      for (let i = 1; i <= this.pageCount; i++) {
        pages.push(i);
      }
      return pages;
    },
  },
  methods: {
    onPageChange(page) {
      console.log(page);
      this.currentPage = page;
    },

    setFavorite(favMovie) {
      let index = this.fav.indexOf(favMovie);
      if (this.fav.some((e) => e == favMovie)) {
        this.fav.splice(index, 1);
      } else {
        this.fav.splice(0, 0, favMovie);
      }
      console.log(this.fav);
    },

    addedtoCart() {
      this.addedtocart = true;
      setTimeout(() => {
        this.addedtocart = false;
      }, "1000");
    },
  },
};
</script>

<style scoped>
.truncate {
  display: inline-block;
  max-width: 100%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
