<template>
    <div>
    <div class="header inset-squish-16">
        <h1>{{ name }}</h1>
    </div>
    <p class="breadcrumb inset-squish-16"><nuxt-link to="/">Home</nuxt-link> / {{ name }}</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: this.$route.params.id
        }
    }
}
</script>

<style scoped>
.header {
    background-color: var(--color-background-default-light);
    height: 30vh;
    max-height: 300px;
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>