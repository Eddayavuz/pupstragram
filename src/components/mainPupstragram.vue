<script>
import FavPups from "./favPups.vue"
export default {
  components: {
    FavPups,
  },
  data() {
    return {
      currentDogUrl: null,
      favs: []
    };
  },
  methods: {
    loadDog: async function () {
      const response = await fetch("https://dog.ceo/api/breeds/image/random");
      const asJson = await response.json();
      this.currentDogUrl = asJson.message;
      console.log(this.currentDogUrl);
    },
    addFav: function () {
      if (!this.favs.includes(this.currentDogUrl)) {
        this.favs.push(this.currentDogUrl);
      }
      this.loadDog();
    },
    removeFav: function (dog) {
      this.favs = this.favs.filter(item => item !== dog);
    },
  },
  created() {
    this.loadDog();
  },
  mounted() {
    if (localStorage.favs) {
      this.favs = JSON.parse(localStorage.favs);
    }
  },
  watch: {
    favs(updatedFavs) {
      localStorage.favs = JSON.stringify(updatedFavs);
    }
  }
};

</script>
<template>
  <div id="app">
    <h1 class="title">Pupstagram</h1>
    <img :src="currentDogUrl" alt="A Good Boy" class="card" />
    <button class="nope" @click="loadDog">next</button>
    <button class="fav" @click="addFav">fav</button>
    <div class="favs">
      <h2>Favorites</h2>
      <ul class="favsList">
        <li v-for="dog in favs" :key="dog.id" class="favsItem">
          <img :src="dog" class="favsImg" alt="A favorite dog" />
          <button class="remove" @click="removeFav(dog)">remove</button>
        </li>
      </ul>
    </div>
  </div>

</template>