<template>
    <div>
        <SearchBar @search="fetchAnime" />
        <Card :animeList="animeList" />
    </div>
</template>

<script>
import SearchBar from './components/SearchBar.vue';
import Card from './components/card.vue';

export default {
    components: {
        SearchBar,
        Card,
    },
    data() {
        return {
            animeList: [],
        };
    },
    methods: {
        async fetchAnime(query) {
            try {
                const url = new URL('https://api.jikan.moe/v4/anime');
                const params = { sfw: 'false', q: query };
                url.search = new URLSearchParams(params).toString();

                const response = await fetch(url);
                const data = await response.json();
                this.animeList = data.data;
            } catch (error) {
                console.error('Error fetching anime:', error);
            }
        },
    },
};
</script>