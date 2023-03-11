<template>
  <div>
    <div class="grid grid-cols-2 justify-items-center relative">
      <div
        class="flex sticky top-12 flex-row object-cover h-96 w-96 max-w-md border-2 border-slate-700 rounded-lg"
      >
        <button
          class="absolute top-1/2 bg-black h-12 text-white rounded-l-lg rotate-180"
          @click="currentimg--"
          :disabled="currentimg == 0"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-6 h-6"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M11.25 4.5l7.5 7.5-7.5 7.5m-6-15l7.5 7.5-7.5 7.5"
            />
          </svg>
        </button>
        <img
          v-for="img in product.images"
          v-show="product.images.indexOf(img) == currentimg"
          class="cursor-pointer m-auto border rounded-lg"
          :src="imagePrefix + img"
          alt=""
        />
        <button
          class="absolute top-1/2 right-0 bg-black h-12 text-white rounded-l-lg"
          @click="currentimg++"
          :disabled="currentimg == product.images.length - 1"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-6 h-6"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M11.25 4.5l7.5 7.5-7.5 7.5m-6-15l7.5 7.5-7.5 7.5"
            />
          </svg>
        </button>
      </div>
      <div class="flex flex-col m-2 justify-around">
        <div>
          <h1
            class="font-bold text-2xl text-left border bg-slate-100 p-2 rounded-lg mt-2"
          >
            {{ product.title }}
          </h1>
        </div>
        <div class="flex flex-row justify-around">
          <div
            class="my-1 w-1/6 max-w-sm bg-stone-800 px-3 py-2 rounded-xl text-lg font-bold text-white flex flex-row items-center justify-center"
          >
            <p>{{ product.price.showPrice }}</p>
            <P class="ml-2">IQD</P>
          </div>
          <div
            class="bg-orange-500 my-1 w-1/6 max-w-sm px-3 py-2 rounded-xl text-lg font-bold text-white"
          >
            {{ product.stock }} Pcs Left
          </div>
        </div>
        <div
          @click="addedtoCart()"
          v-show="product.stock > 0"
          class="cursor-pointer font-semibold w-1/2 m-auto bg-indigo-400 text-white rounded-xl py-4 flex flex-row items-center justify-center"
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
        <!-- <h1 class="font-bold text-xl bg-slate-100 p-2 rounded-lg">
          {{ product.shortDescription }}
        </h1> -->

        <div
          class="text-2xl text-left m-auto border bg-slate-100 p-5 rounded-lg"
        >
          <h1 class="text-center">Specs</h1>
          <div v-html="product.longDescription"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imagePrefix: "https://ardunic-images.s3.eu-central-1.amazonaws.com/",
      product: {},
      currentimg: 0,
      specs: "",
    };
  },
  created() {
    fetch(`https://api.ardunic.com/v1/product/${this.$route.params.id}`)
      .then((response) => response.json())
      .then((product) => {
        this.product = product.data;
        console.log(this.product.longDescription);
      });
  },
};
</script>
