<template>
  <div id="app">
    <img width="100" src="./assets/logo.png">
    <SimpleCarousel :height="200" :trim="true">
      <div class="banner" v-for="(item, index) in items" :key="index">
        <div class="banner-item"></div>
      </div>
    </SimpleCarousel>
  </div>
</template>

<script>
import SimpleCarousel from "./components/Carousel";

export default {
  name: "App",
  components: {
    SimpleCarousel
  },

  data: () => ({
    items: 8
  })
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

body {
  margin-top: 60px;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 1400px;
  text-align: center;
  color: #2c3e50;
  margin: 0 auto;
  padding: 0 20px;
}

button {
  outline: none;
  border: none;
}

.banner {
  min-width: 25%;
  height: 100%;
  padding: 0 5px;

  .banner-item {
    display: flex;
    justify-content: center;
    align-items: center;
    min-width: 100%;
    height: 100%;
    border-radius: 3px;
    /* background: url("https://cdn.fstoppers.com/styles/large-16-9/s3/lead/2018/12/cat-parody-instagram-removal.jpg"); */
    background: url("https://placekitten.com/200/250");
    background-size: cover;
    background-position: center center;
  }

  @media only screen and (max-width: 1100px) {
    width: 33.33333%;
    min-width: 33.33333%;
  }

  @media only screen and (max-width: 700px) {
    width: 50%;
    min-width: 50%;
  }

  @media only screen and (max-width: 500px) {
    width: 100%;
    min-width: 100%;
  }
}
</style>
