<template>
    <div class="app">
        <header>
            <h1>The<strong>Movie</strong>Database</h1>
            <form class="search-box" @submit.prevent="handleSearch">
                <input
                    type="search"
                    class="search-field"
                    placeholder="Search for a movie..."
                    required
                    v-model="search_query"
                />
            </form>
        </header>
        <main>
            <div v-if="isActive" :class="{ loader: isActive }"></div>
            <div v-else>
                <div class="cards" v-if="movieList.length">
                    <movieCard
                        v-for="movie in movieList"
                        :key="movie.imdbID"
                        :movie="movie"
                    />
                </div>
                <div class="no-results" v-else>
                    <h3>Search your movie above...</h3>
                </div>
            </div>
        </main>
    </div>
</template>

<script>
import { ref } from "vue";
import movieCard from "./components/movieCard";
import env from "../env";

export default {
    components: {
        movieCard,
    },
    setup() {
        const search_query = ref("");
        const movieList = ref([]);
        const isActive = ref(false);

        const handleSearch = async () => {
            // Need to add loader effect until data gets retrived
            isActive.value = true;

            movieList.value = await fetch(
                `https://www.omdbapi.com/?apikey=${env.apikey}&s=${search_query.value}`
            )
                .then((res) => res.json())
                .then((data) =>
                    !data.Search
                        ? confirm("Invalid / no matching data found!")
                        : data.Search
                );
            isActive.value = false;
            console.log(movieList);

            search_query.value = "";
        };
        // isActive.value = false;

        return {
            search_query,
            movieList,
            isActive,
            handleSearch,
        };
    },
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;

    font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
        "Lucida Sans", Arial, sans-serif;
}

a {
    text-decoration: none;
}

header {
    padding: 50px 0;
}

header > h1 {
    color: #888;
    font-size: 42px;
    font-weight: 400;
    text-align: center;
    text-transform: uppercase;
    margin-bottom: 30px;
}

header > h1 > strong {
    color: #313131;
}

header > h1:hover {
    color: #313131;
}

.search-box {
    display: flex;
    justify-content: center;
    padding: 0 30px;
}

.search-field {
    appearance: none;
    background: none;
    border: none;
    outline: none;
    border-radius: 8px;

    background-color: #f3f3f3;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);

    display: block;
    width: 100%;
    max-width: 600px;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    transition: 0.4s;
}

.search-field::placeholder {
    color: #aaa;
}

.search-field:focus,
.search-field:valid {
    color: #fff;
    background-color: #313131;
    box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
}

main {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 30px;
}

.cards {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
}

.no-results {
    text-align: center;
}

.loader {
    border: 16px solid #f3f3f3;
    border-top: 16px solid #3498db;
    border-radius: 50%;
    width: 36px;
    height: 36px;
    margin: 0 auto;
    animation: spin 2s linear infinite;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}

@media screen and (max-width: 688px) {
    .cards {
        display: unset;
        flex-wrap: unset;
    }
}
</style>
