<template>
    <div v-for="anime in animeList" :key="anime.mal_id">
        <h2 class="bg-red">{{ anime.title }}</h2>
        <img :src="anime.images.jpg.image_url" alt="Anime image">
        <p>{{ anime.synopsis }}</p>
    </div>
</template>

<script>
export default {
    props: ['animeList'],
};
</script>