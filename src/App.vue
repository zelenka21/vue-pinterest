<template>
  <div id="app">
    <img class="logo" alt="Zelenka" src="./assets/logo.png">
   <div class="container">
      <div class="button-wrapper">
        <button class="btn" @click="searchUnsplash('car')">Cars</button>
         <button class="btn" @click="searchUnsplash('bike')">Bikes</button>
           <button class="btn" @click="searchUnsplash('plane')">Planes</button>
      </div>


      <stack
              :column-min-width="300"
              :gutter-width="15"
              :gutter-height="15"
              monitor-images-loaded
      >
        <stack-item
                v-for="(image, i) in images"
                :key="i"
                style="transition: transform 300ms"
        >
          <img :src="image.urls.small" :alt="image.alt_description" />
        </stack-item>
      </stack>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { Stack, StackItem } from "vue-stack-grid";
export default {
  name: "app",
  components: {
    Stack,
    StackItem
  },
  data: () => ({
    images: []
  }),
  methods: {
    searchUnsplash(topic) {
      this.images = [];
      axios
        .get(
          `https://api.unsplash.com/search/photos?query=${topic}&per_page=20`,
          {
            headers: {
              Authorization:
                "Client-ID a6230bba66b123cf708eea39b974041b2be29fef713b30538a84d84de8df89a6",
              "Accept-Version": "v1"
            }
          }
        )
        .then(response => {
          this.images = response.data.results;
        })
        .catch(() => {
          this.images = [];
        });
    }
  }
};

</script>

<style>
#app {
  font-family: 'Roboto', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  width: 80vw;
  margin: 0 auto;
}
.button-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
}
.btn {
  font-size: 18px;
  background-color: #334FFF;
  color: white;
  margin: 25px 5px 10px;
  padding:  10px 30px;
}
img {
  width: 100%;
  height: auto;
  border-radius: 12px;
}
.logo {
  width: 40vw;
  border-radius: 0;
}
</style>
