<template>
    <v-container v-if="loading">
        <div class="text-xs-center">
            <v-progress-circular indeterminate :size="150" :width="8" color="green"></v-progress-circular>
        </div>
    </v-container>

    <v-container v-else-if="noData">
        <div class="text-xs-center">
            <h2>No Movie in API with {{this.name}}</h2>
        </div>
    </v-container>

    <v-container v-else grid-list-xl>
        <v-layout wrap>
            <v-flex md3 align-stretch v-for="(item, index) in movieResponse" :key="index" mb-2>
                <v-card
                    style="display:flex;flex-direction:column; justify-content:space-between;"
                    height="100%"
                >
                    <v-img max-height="200px" :src="item.Poster" aspect-ratio="1"></v-img>

                    <v-card-title primary-title>
                        <div style="width:100%;">
                            <h4>{{item.Title}}</h4>
                            <hr style="margin:8px 0;" />
                            <p>
                                <small>Year: {{item.Year}}</small>
                            </p>
                            <p>
                                <small>Type: {{item.Type}}</small>
                            </p>
                            <p>
                                <small>IMDB-id: {{item.imdbID}}</small>
                            </p>
                        </div>
                    </v-card-title>

                    <v-card-actions class="justify-center">
                        <v-btn width="100%" @click="singleMovie(item.imdbID)">View</v-btn>
                    </v-card-actions>
                </v-card>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
import movieApi from "@/services/MovieApi";

export default {
    props: ["name"],
    data() {
        return {
            movieResponse: [],
            loading: true,
            noData: false
        };
    },
    methods: {
        singleMovie(id) {
            this.$router.push("/movie/" + id);
        },

        fetchResult(value) {
            movieApi
                .fetchMovieCollection(value)
                .then(response => {
                    if (response.Response === "True") {
                        this.movieResponse = response.Search;
                        this.loading = false;
                        this.noData = false;
                    } else {
                        this.noData = true;
                        this.loading = false;
                    }
                })
                .catch(error => {
                    console.log(error);
                });
        }
    },
    mounted() {
        this.fetchResult(this.name);
    },
    watch: {
        name(value) {
            this.fetchResult(value);
        }
    }
};
</script>

<style lang="scss" scoped>
.v-progress-circular {
    margin: 1rem;
}
</style>