<template>
    <q-card class="movie-card">
      <q-card-section>
        <q-img :src="getFullPosterPath(movie.poster_path)" alt="Imagen del movie" />
      </q-card-section>
      <q-card-section>
        <q-card-title>{{ movie.title }}</q-card-title>
        <div class="text-h6 font-weight-bold">Rating: {{ movie.vote_average }}</div>
        <div class="text-h6 font-weight-bold">Votos: {{ movie.vote_count }}</div>
      </q-card-section>
    </q-card>
  </template>
  
  <style>
  .movie-card {
    width: 300px;
  }
  .movie-card q-card-section {
    height: 100px;
    overflow: hidden;
  }
  </style>
  
  <script>
  export default {
    name: "MovieItem",
    props: {
      movie: {
        type: Object,
        required: true,
      },
    },
    data() {
      return {
        urlImage: "https://gebelesebeti.ge/front/asset/img/default-product.png",
      };
    },
    methods: {
        getFullPosterPath(posterPath) {
            const baseURL = 'http://image.tmdb.org/t/p/w500/'; // Cambia a tu URL base
            return baseURL + posterPath;
        },
    },
  };
  </script>