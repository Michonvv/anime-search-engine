<template>
    <div class="grid">
      <article v-for="anime in animeList" :key="anime.mal_id" class="card">
        <div class="card-image">
          <img :src="anime.images.jpg.image_url" :alt="anime.title">
          <div class="card-badge" v-if="anime.score">
            {{ anime.score.toFixed(1) }}
          </div>
        </div>
        <div class="card-content">
          <h3 class="card-title">{{ anime.title }}</h3>
          <div class="card-meta">
            <span class="meta-item">{{ anime.type }}</span>
            <span class="meta-item" v-if="anime.episodes">{{ anime.episodes }} eps</span>
            <span class="meta-item">{{ anime.year }}</span>
          </div>
          <p class="card-description">{{ truncateSynopsis(anime.synopsis) }}</p>
          <div class="card-tags">
            <span v-for="genre in anime.genres.slice(0, 3)" :key="genre.mal_id" class="tag">
              {{ genre.name }}
            </span>
          </div>
        </div>
      </article>
    </div>
  </template>
  
  <script>
  export default {
    props: ['animeList'],
    methods: {
      truncateSynopsis(synopsis) {
        if (!synopsis) return 'No synopsis available';
        return synopsis.length > 150 ? synopsis.slice(0, 150) + '...' : synopsis;
      }
    }
  };
  </script>
  
  <style>
  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1.5rem;
    padding: 1rem 0;
  }
  
  .card {
    background: var(--background);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    overflow: hidden;
    transition: border-color 0.2s, transform 0.2s;
  }
  
  .card:hover {
    border-color: var(--ring);
    transform: translateY(-2px);
  }
  
  .card-image {
    position: relative;
    aspect-ratio: 2/3;
    overflow: hidden;
  }
  
  .card-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  
  .card-badge {
    position: absolute;
    top: 0.75rem;
    right: 0.75rem;
    background: var(--foreground);
    color: var(--background);
    padding: 0.25rem 0.5rem;
    border-radius: var(--radius);
    font-size: 0.75rem;
    font-weight: 500;
  }
  
  .card-content {
    padding: 1rem;
  }
  
  .card-title {
    font-size: 1rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
    color: var(--foreground);
    display: -webkit-box;
    -webkit-line-clamp: 1;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  
  .card-meta {
    display: flex;
    gap: 0.75rem;
    margin-bottom: 0.75rem;
  }
  
  .meta-item {
    color: var(--muted);
    font-size: 0.75rem;
  }
  
  .card-description {
    color: var(--muted-foreground);
    font-size: 0.875rem;
    margin-bottom: 1rem;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  
  .card-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  
  .tag {
    background: var(--input);
    color: var(--muted);
    font-size: 0.75rem;
    padding: 0.25rem 0.5rem;
    border-radius: var(--radius);
    white-space: nowrap;
  }
  </style>