<template>
    <form @submit.prevent="$emit('search', searchQuery)" class="search-form">
      <div class="search-wrapper">
        <input 
          type="text" 
          v-model="searchQuery" 
          placeholder="Search anime..."
          class="search-input"
        >
        <button type="submit" class="search-button">Search</button>
      </div>
    </form>
  </template>
  
  <script>
  export default {
    data() {
      return {
        searchQuery: '',
      };
    },
  };
  </script>
  
  <style>
  .search-form {
    width: 100%;
    max-width: 600px;
    margin: 0 auto;
  }
  
  .search-wrapper {
    display: flex;
    gap: 0.5rem;
  }
  
  .search-input {
    flex: 1;
    padding: 0.625rem 0.75rem;
    background-color: var(--background);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    font-size: 0.875rem;
    color: var(--foreground);
    transition: border-color 0.2s;
  }
  
  .search-input:focus {
    outline: none;
    border-color: var(--ring);
  }
  
  .search-button {
    padding: 0.625rem 1rem;
    background-color: var(--foreground);
    color: var(--background);
    border: none;
    border-radius: var(--radius);
    font-size: 0.875rem;
    font-weight: 500;
    cursor: pointer;
    transition: opacity 0.2s;
  }
  
  .search-button:hover {
    opacity: 0.9;
  }
  </style>