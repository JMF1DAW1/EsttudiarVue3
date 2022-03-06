<template>
    <div>
        <!-- TODO: Crear componente GitHubRepos -->
        <div v-for="(item, index) in repolist" :key="index">
            <div v-if="index %2 == 0">
                {{ index }} Nombre del repositorio: {{ item.name }}
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'GitHubRepos',
    props: [
        'repolist'
    ],
}
</script>

<style scoped>
</style>
