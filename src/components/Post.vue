<template>
    <div v-for="post in posts" :key="post">
        <span>{{ post.name }}</span>
</template>

<script>
export default {
    data() {
        return {
            posts: []
        }
    },
    async created() {
        const res = await fetch('https://swapi.dev/api/planets/');
        const { results } = await res.json();
        this.posts = results;
    }
}