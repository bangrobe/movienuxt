<template>
  <div>
    <h1>{{ movieData.title }}</h1>
    <div>
      <img :src="`https://image.tmdb.org/t/p/w1280/${movieData.posterURL}`" />
    </div>
  </div>
</template>

<script>
const apiKey = "04c35731a5ee918f014970082a0088b1";
export default {
  async asyncData({ params, $axios }) {
    const res = await $axios.$get(
      `https://api.themoviedb.org/3/movie/${params.id}?api_key=${apiKey}`
    );
    return {
      movieData: {
        title: res.original_title,
        posterURL: res.backdrop_path,
        releaseDate: res.release_date,
        imdbRating: res.vote_average
      }
    };
  }
};
</script>

<style></style>
