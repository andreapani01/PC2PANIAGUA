<template>
    <page-container>
        <div class="q-pa-md example-column-equal-width">
            <div class="column" style="height: 150px; max-width: 300px">
                <div class="col">
                    <div class="q-gutter-x-xs q-gutter-y-lg col column">
                        <div class="col4 col-sm-6">
                            <q-select outlined v-model="categorySelected" :options="categories" label="Categoria" option-value="id" option-label="name"/>
                        </div>
                        <div class="col4 col-sm-6">
                            <q-card>
                                <q-card-section>
                                    <div class="text-overline">Rating</div>
                                    <q-input class="rate" v-model="voteAverageGte" outlined type="number" label="Mayor que" />
                                </q-card-section>
                                <q-separator />
                                <q-card-section>
                                    <q-input class="rate" v-model="voteAverageLte" outlined type="number" label="Menor que" />
                                </q-card-section>
                            </q-card>
                            
                        </div>
                        <div class="col4 col-sm-6">
                            <q-btn
                                label="Filtrar"
                                type="submit"
                                color="primary"
                                unelevated
                                class="full-width q-mt-md"
                                @click="emitFilter"
                            />
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </page-container>
</template>
<script>
export default {
  name: "MovieFilter",
  emits: ["categoriaCambiada"],
  data() {
    return {
      minimo: 0,
      maximo: 0,
      model: null,
      categories: [],
      adultbool: ["true", "false"],
      categorySelected: null,
      includeAdult: null,
      voteAverageGte: null,
      voteAverageLte: null,
    };
  },
  mounted() {
    this.cargarCategorias();
  },
  methods: {
    // Listar las categorias
    cargarCategorias() {
      let endpointURL = "/3/genre/movie/list";
      let token = "eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1ZjU4ZTA1MTA0YTQxYjllNTA5MDk3NTg1M2M1YTFmMyIsIm5iZiI6MTczMjE2MDIwNC4yMTc5MTg2LCJzdWIiOiI2NzNlYWEzOWFkZTkzMTBmM2ZkZjk2N2MiLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.clCTVlj5o2F5sqoijyLenWsEJtav4xUIAkf9zswfKDA";
      let headers = {
        headers: {
          Authorization: "Bearer " + token,
        },
      };
      this.$api_movie.get(endpointURL, headers)
        .then(response => {
            console.log(JSON.stringify(response.data));
            this.categories = response.data.genres;
        })
        .catch(error => {
          console.error(error);
        });
    },
    emitFilter() {
        console.log("Emitiendo filtros:", {
            voteAverageLteCambiada: this.voteAverageLte,
            categoriaCambiada: this.categorySelected,
            voteAverageGteCambiada: this.voteAverageGte,
      });
      // Emitir evento con los valores de los filtros
      this.$emit("applyFilter", {
        voteAverageLteCambiada: this.voteAverageLte,
        categoriaCambiada: this.categorySelected,
        voteAverageGteCambiada: this.voteAverageGte,
      });
    },
  }
};
</script>