  
<script>
import axios from 'axios';

export default {
    data() {
        return {
            searchQuery: '',
            movies: []
        };
    },
    methods: {
        searchMovies() {
            const apiKey = '7fcd9b8a5e68eff972fcf8c6c7642af0';
            const url = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${this.searchQuery}`;

            axios.get(url)
                .then(response => {
                    this.movies = response.data.results;

                })
                .catch(error => {
                    console.error("Errore nella ricerca dei film:", error);
                });
        }
    }
};
</script>
  
<template>
    <div>
        <div>
            <input type="text" v-model="searchQuery" @keyup.enter="searchMovies" placeholder="Cerca un film...">
            {{ " " }} 
            <button @click="searchMovies">Cerca</button>
        </div>

        <div v-if="movies.length">
            <h2>Risultati della ricerca:</h2>
            <ul>
                <li v-for="movie in movies" :key="movie.id">
                    <h3>{{ movie.title }}</h3>
                    <p>Titolo Originale: {{ movie.original_title }}</p>
                    <p>Lingua: {{ movie.original_language.toUpperCase() }}</p>
                    <p>Voto: {{ movie.vote_average }}/10</p>
                </li>
            </ul>
        </div>
    </div>
</template>



<style scoped>
ul {
    list-style: none;
}
</style>
  