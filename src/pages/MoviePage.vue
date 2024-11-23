<template>
    <q-page>
        <div class="q-pa-md example-row-stacked-to-horizontal">
        <div class="row">
          <div class="col-12 col-md-4">
            <MovieFilter @applyFilter="handleFilter"/>
          </div>

          <div class="col-12 col-md-8">
            <MovieList ref="movieList"/>
          </div>
        </div>
      </div>
    </q-page>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  import MovieFilter from "components/app/MovieFilter.vue"
  import MovieList from "components/app/MovieList.vue"
  
  export default defineComponent({
    name: 'MoviePage',
    components: {MovieFilter, MovieList},
    data() {
        return {
        categoriaSeleccionada: null
        }
    },
    methods: {
        // updateCategoria(categoria) {
        // console.log("La categoria recibida es: " + categoria);
        // this.categoriaSeleccionada = categoria;
        // },
        handleFilter(filterValues) {
        // Llama a la función fetchMovies del componente MovieList
        console.log("Los datos recibidos son: " + filterValues);
        console.log("voteAverageLteCambiada:", filterValues.voteAverageLteCambiada);
        console.log("categoriaCambiada:", filterValues.categoriaCambiada);
        console.log("voteAverageGteCambiada:", filterValues.voteAverageGteCambiada);
        this.$refs.movieList.fetchMovies(filterValues);
        },
    }
  });
  </script>