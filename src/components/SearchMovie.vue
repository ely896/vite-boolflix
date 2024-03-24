  
<script>
import axios from 'axios';


export default {

    name: 'SearchMovie',
    data() {
        return {
            searchQuery: '',
            results: []
        };
    },
    methods: {
        searchMovies() {
            const apiKey = '7fcd9b8a5e68eff972fcf8c6c7642af0';
            return axios.get(`https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${this.searchQuery}`);
        },
        searchTvShows() {
            const apiKey = '7fcd9b8a5e68eff972fcf8c6c7642af0';
            return axios.get(`https://api.themoviedb.org/3/search/tv?api_key=${apiKey}&query=${this.searchQuery}`);
        },
        searchAll() {
            Promise.all([this.searchMovies(), this.searchTvShows()])
                .then(([moviesResponse, tvShowsResponse]) => {
                    const movies = moviesResponse.data.results.map(movie => ({
                        ...movie,
                        title: movie.title,
                        original_title: movie.original_title,
                        type: 'movie'
                    }));
                    const tvShows = tvShowsResponse.data.results.map(tvShow => ({
                        ...tvShow,
                        title: tvShow.name,
                        original_title: tvShow.original_name,
                        type: 'tv'
                    }));
                    this.results = [...movies, ...tvShows];
                })
                .catch(error => {
                    console.error("Errore nella ricerca:", error);
                });
        },
        calculateStars(vote_average) {
            return Math.ceil(vote_average / 2);
        },
        getImageUrl(posterPath) {
            return `https://image.tmdb.org/t/p/w342${posterPath}`;
        },
        getCountryCode(languageCode) {
            // language codes to country codes
            const languageToCountry = {
                en: 'gb', // Inglese a Gran Bretagna
                it: 'it', // Italiano a Italia
                fr: 'fr', // Francese a Francia
                es: 'es', // Spagnolo a Spagna
                de: 'de', // Tedesco a Germania
                ru: 'ru', // Russo a Russia
                ja: 'jp', // Giapponese a Giappone
                ko: 'kr', // Coreano a Corea del Sud
                zh: 'cn', // Cinese a Cina
                ar: 'sa', // Arabo a Arabia Saudita
                pt: 'pt', // Portoghese a Portogallo
                nl: 'nl', // Olandese a Paesi Bassi
                sv: 'se', // Svedese a Svezia
                no: 'no', // Norvegese a Norvegia
                da: 'dk', // Danese a Danimarca
                fi: 'fi', // Finlandese a Finlandia
                pl: 'pl', // Polacco a Polonia
                hu: 'hu', // Ungherese a Ungheria
                cs: 'cz', // Ceco a Repubblica Ceca
                sk: 'sk', // Slovacco a Slovacchia
                sl: 'si', // Sloveno a Slovenia
                el: 'gr', // Greco a Grecia
                tr: 'tr', // Turco a Turchia
                he: 'il', // Ebraico a Israele
                th: 'th', // Tailandese a Tailandia
                hi: 'in', // Hindi a India
                bn: 'bd', // Bengalese a Bangladesh
            };

            return languageToCountry[languageCode] || 'us'; // Provides a default country code
        },
    }
};
</script>
   
<template>
    <div>
        <input type="text" v-model="searchQuery" @keyup.enter="searchAll" placeholder="Cerca un film o una serie TV..."> {{ " " }}
        <button @click="searchAll">Cerca</button>

        <div v-if="results.length">
            <h2>Risultati della ricerca:</h2>

            <div v-for="result in results" :key="result.id" class="result">
                <!-- <h3>{{ result.title }}</h3> -->
                <img :src="getImageUrl(result.poster_path)" alt="Copertina" class="cover-image">
                <div v-if="result.type === 'movie'">
                    <h3>Titolo del film: {{ result.title }}</h3>
                </div>
                <div v-if="result.type === 'tv'">
                    <h3>Titolo serie TV: {{ result.title }}</h3>
                </div>
                <p>Titolo Originale: {{ result.original_title }}</p>
                <p>Lingua: <span :class="'fi fi-' + getCountryCode(result.original_language)"></span></p>
                <p>Voto: {{ calculateStars(result.vote_average) }}/5</p>
                <div>
                    <!-- Stelle piene -->
                    <i v-for="star in calculateStars(result.vote_average)" :key="star" class="fas fa-star"></i>
                    <!-- Stelle vuote -->
                    <i v-for="emptyStar in 5 - calculateStars(result.vote_average)" :key="emptyStar + 5"
                        class="far fa-star"></i>
                </div>

            </div>
        </div>
    </div>
</template>

<style scoped>
ul {
    list-style: none;
}

.result {
    margin-bottom: 20px;
}

.cover-image {
    width: 100%;
    max-width: 342px;
    height: auto;
}
</style>