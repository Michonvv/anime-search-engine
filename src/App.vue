<template>
    <div class="app-container">
      <header class="header">
        <h1 class="header-title">Anime Search</h1>
        <SearchBar @search="fetchAnime" />
      </header>
      
      <div class="filters-container">
        <select v-model="selectedType" @change="applyFilters" class="select">
          <option value="">All Types</option>
          <option value="TV">TV Series</option>
          <option value="Movie">Movie</option>
          <option value="OVA">OVA</option>
        </select>
        <select v-model="sortBy" @change="applyFilters" class="select">
          <option value="score">Sort by Rating</option>
          <option value="title">Sort by Title</option>
          <option value="year">Sort by Year</option>
        </select>
      </div>
  
      <div class="content">
        <div v-if="loading" class="loading">
          <div class="loading-spinner"></div>
          <span>Loading results...</span>
        </div>
        <p v-if="error" class="error">{{ error }}</p>
        <Card :animeList="filteredAnimeList" />
      </div>
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
        loading: false,
        error: null,
        selectedType: '',
        sortBy: 'score',
      };
    },
    computed: {
      filteredAnimeList() {
        let filtered = [...this.animeList];
        
        if (this.selectedType) {
          filtered = filtered.filter(anime => anime.type === this.selectedType);
        }
        
        switch(this.sortBy) {
          case 'score':
            filtered.sort((a, b) => b.score - a.score);
            break;
          case 'title':
            filtered.sort((a, b) => a.title.localeCompare(b.title));
            break;
          case 'year':
            filtered.sort((a, b) => b.year - a.year);
            break;
        }
        
        return filtered;
      }
    },
    methods: {
      async fetchAnime(query) {
        this.loading = true;
        this.error = null;
        try {
          const url = new URL('https://api.jikan.moe/v4/anime');
          const params = { sfw: 'false', q: query };
          url.search = new URLSearchParams(params).toString();
  
          const response = await fetch(url);
          const data = await response.json();
          this.animeList = data.data;
        } catch (error) {
          this.error = 'Error fetching anime. Please try again.';
          console.error('Error fetching anime:', error);
        } finally {
          this.loading = false;
        }
      },
      applyFilters() {
        // The computed property will handle the filtering
      }
    },
  };
  </script>
  
  <style>
/* Global Reset and Variables */
:root {
  --background: #ffffff;
  --foreground: #0f172a;
  --muted: #64748b;
  --muted-foreground: #94a3b8;
  --border: #e2e8f0;
  --input: #e2e8f0;
  --ring: #0f172a;
  --radius: 0.5rem;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: var(--background);
  color: var(--foreground);
  line-height: 1.5;
}

.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1.5rem;
}

.header {
  margin-bottom: 2rem;
}

.header-title {
  font-size: 2rem;
  font-weight: 600;
  letter-spacing: -0.025em;
  margin-bottom: 1.5rem;
}

.filters-container {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
}

.select {
  appearance: none;
  background-color: var(--background);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 0.5rem 2.5rem 0.5rem 0.75rem;
  font-size: 0.875rem;
  line-height: 1.25rem;
  color: var(--foreground);
  cursor: pointer;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='24' height='24' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='m6 9 6 6 6-6'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 0.5rem center;
  background-size: 1.5em 1.5em;
  min-width: 150px;
}

.select:focus {
  outline: none;
  border-color: var(--ring);
  ring: 2px var(--ring);
}

.loading {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  padding: 2rem;
  color: var(--muted);
}

.loading-spinner {
  width: 1.25rem;
  height: 1.25rem;
  border: 2px solid var(--muted);
  border-top-color: var(--foreground);
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.error {
  color: #ef4444;
  text-align: center;
  padding: 1rem;
  border-radius: var(--radius);
  background: #fee2e2;
  margin: 1rem 0;
}
</style>
