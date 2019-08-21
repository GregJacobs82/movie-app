<template>
    <v-app>
        <v-app-bar app>
            <v-toolbar-title class="headline text-uppercase">
                <span>Greg's</span>
                <span class="font-weight-light">Movie App</span>
            </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-flex xs12 sm6 md3>
                <v-text-field label="Movie Name" v-model="searchString"></v-text-field>
            </v-flex>
            <v-btn text :disabled="!dataAvailable" @click="searchMovie">
                <span>Search</span>
            </v-btn>
        </v-app-bar>

        <v-content>
            <router-view></router-view>
        </v-content>
    </v-app>
</template>

<script>
export default {
    name: "App",
    data() {
        return {
            searchString: ""
        };
    },
    methods: {
        searchMovie() {
            this.$router.push("/search/" + this.searchString);
            this.searchString = "";
        }
    },
    computed: {
        dataAvailable() {
            return this.searchString !== null && this.searchString !== "";
        }
    }
};
</script>
