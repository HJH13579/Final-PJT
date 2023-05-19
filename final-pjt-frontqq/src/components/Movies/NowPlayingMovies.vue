<template>
  <div class="container mt-3">
    <div class="row">
       <!-- <MovieDetailView
      v-for="movie in movies" 
      :key="movie.id" 
      
      :movie="movie"
    /> -->
  
      <carousel :perPage="7" :navigationEnabled="true" :paginationEnabled="false" :loop="true">
        <slide MovieDetailView v-for="movie in movies" :key="movie.id">
          
          <div class="card mb-2">
            <img class="card-img-top" :src="getImgUrl(movie)" alt="Card image cap" @click="goTotmdb">
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
const NOW_PLAYING_URL = `https://api.themoviedb.org/3/movie/now_playing?api_key=${API_KEY}&language=ko-KR`;
// const ID_URL = 'https://www.themoviedb.org/movie/'
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
    this.getNowPlayingMovies()
  },
  methods: {
    //   goTotmdb() {
    //   window.open(this.tmdbUrl, "_blank")
    // },
    // 새로운 창 띄워서 tmdb 사이트로 보내는 코드입니다.
    //   movedetail(){
    //   this.$router.push({
    //     path: '/moviedetail',
    //     query: {movie: this.movie},

    //     })

    // },
   // 디테일 라우터로 push하는 코드입니다.

    getNowPlayingMovies() {
      axios({
        method: 'get',
        url: NOW_PLAYING_URL,
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
  computed: {
    tmdbUrl(){
      return  `${ID_URL}` + this.movie.id
    },
  }
};
</script>

<style>
.card-img-top {
  height: 100%;
  width: 100%;
  object-fit: cover;
}

</style>
