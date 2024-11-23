<template>
    <q-card flat bordered>
        <q-card-section>
            <div class="text-h6">Movies</div>
        </q-card-section>
        <q-separator inset dark />
    </q-card>
    <div class="movie-list">
        <div class="movie-grid">
            <div class="movie-item" v-for="item in moviesFilter" :key="item.id">
                <MovieItem :movie="item" />
            </div>
        </div>
    </div>
</template>
<style>
.movie-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
}
</style>
<script>
import MovieItem from "components/app/MovieItem.vue";

export default {
  name: "MovieList",
  components: { MovieItem },
  computed: {
    filtrarMoviePorCategoria() {
      return this.movies.filter(
        (movie) => movie.genre_ids.includes(this.categoriaFiltrada)
      )
    }
  },
  props: {
    categoriaFiltrada: {
      type: Number,
      default: null
    },
    voteAverageGte: {
      type: Number,
      default: null
    },
    includeAdult: {
      type: Boolean,
      default: null
    },
  },
  data() {
    return {
        moviesFilter: [],
    };
  },
  methods: {

    fetchMovies(filterValues) {
      const voteAverageLteRecibido = filterValues.voteAverageLteCambiada || null;
      const categoriaRecibido = filterValues.categoriaCambiada?.id || null;
      const voteAverageGteRecibido = filterValues.voteAverageGteCambiada || null;

      let endpointURL = "/3/discover/movie";
      let token = "eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1ZjU4ZTA1MTA0YTQxYjllNTA5MDk3NTg1M2M1YTFmMyIsIm5iZiI6MTczMjE2MDIwNC4yMTc5MTg2LCJzdWIiOiI2NzNlYWEzOWFkZTkzMTBmM2ZkZjk2N2MiLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.clCTVlj5o2F5sqoijyLenWsEJtav4xUIAkf9zswfKDA";
      let headers = {
        headers: {
          Authorization: "Bearer " + token,
        },
        params: {
        "with_genres": categoriaRecibido,
        "vote_average.gte": voteAverageGteRecibido,
        "vote_average.lte": voteAverageLteRecibido,
        }
      };

      this.$api_movie
        .get(endpointURL, headers)
        .then((response) => {
          console.log(JSON.stringify(response.data.results));
          this.moviesFilter = response.data.results;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>