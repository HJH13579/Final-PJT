<template>
  <div class="container mt-3">
    <div class="row">
      <carousel :perPage="7" :navigationEnabled="true" :paginationEnabled="false" :loop="true">
        <slide v-for="movie in movies" :key="movie.id">
          <div class="card mb-2">
            <img class="card-img-top" :src="getImgUrl(movie)" alt="Card image cap">
          </div>
        </slide>
      </carousel>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { Carousel, Slide } from 'vue-carousel';

const API_KEY = 'db499efb2cc0ba6f9698b4699f1b762e';
const TOP_RATED_URL = `https://api.themoviedb.org/3/movie/top_rated?api_key=${API_KEY}&language=ko-KR&page=1`

export default {
  name: 'MoviesView',
  components: {
    Carousel,
    Slide,
  },
  data() {
    return {
      movies: [],
    };
  },
  created() {
    this.getTopRatedMovies()
  },
  methods: {
    getTopRatedMovies() {
      axios({
        method: 'get',
        url: TOP_RATED_URL,
      })
        .then((response) => {
          const moviesData = response.data.results;
          this.movies = moviesData;
        });
    },
    getImgUrl(movie) {
      const baseImgUrl = 'https://image.tmdb.org/t/p/w200';
      return baseImgUrl + movie.poster_path;
    },
  },
};
</script>

<style>
.card-img-top {
  height: 100%;
  width: 100%;
  object-fit: cover;
}

</style>
