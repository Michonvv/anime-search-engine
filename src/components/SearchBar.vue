<template>
    <form @submit.prevent="$emit('search', searchQuery)">
        <input type="text" v-model="searchQuery" placeholder="Search anime...">
        <button type="submit">Search</button>
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