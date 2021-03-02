<template>
  <main class="p-10">
    <form>
      <div class="flex justify-center m-auto">
        <input
          v-model="searchTerm"
          type="text"
          placeholder="Search"
          class="rounded-md lg:w-2/12 bg-gray-100 pl-4"
        />
        <button
          @click.prevent="search"
          class="bg-blue-300 p-3 text-2xl rounded-md ml-4 text-white uppercase"
        >
          Search
        </button>
      </div>
    </form>
    <div class="flex flex-wrap justify-around">
      <div
        v-for="img of imgs"
        :key="img.title"
        class="w-64 bg-purple-300 p-3 rounded-md mt-4"
      >
        <img :src="img.poster" alt="img.title" />
        <nuxt-link :to="`${img.movieId}`">
          <h3 class="text-xl font-extrabold">{{ img.title }}</h3>
        </nuxt-link>
        <span> {{ img.date }} </span>
      </div>
    </div>
  </main>
</template>

<script>
const popURL =
  "https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=04c35731a5ee918f014970082a0088b1&page=1";
const bg = "https://image.tmdb.org/t/p/w1280";
const searchURL =
  "https://api.themoviedb.org/3/search/movie?&api_key=04c35731a5ee918f014970082a0088b1&query=";
export default {
  name: "HomePage",
  data() {
    return {
      imgs: [],
      searchTerm: ""
    };
  },
  methods: {
    async search() {
      if (this.searchTerm === "") return;
      const res = await this.$axios.$get(`${searchURL}${this.searchTerm}`);
      console.log(res.results);
      this.imgs = this.parseImgRes(res);
    },
    async fetchPops() {
      const res = await this.$axios.$get(popURL);
      this.imgs = this.parseImgRes(res);
    },
    parseImgRes(movies) {
      return movies.results.reduce((acc, movie) => {
        if (!movie.poster_path) {
          return acc;
        }

        acc.push({
          poster: `${bg}${movie.poster_path}`,
          date: movie.release_date,
          title: movie.title,
          movieId: movie.id
        });

        return acc;
      }, []);
    }
  },
  mounted() {
    this.fetchPops();
  }
};
</script>

<style></style>
