<template>
    <section id="projects" style="height: 100vh; background-color: lightpink;">
        Projects
        <ProjectsCards v-for="project in projects" :key="project.topics_id" :project="project" />
    </section>
</template>

<script>
import ProjectsCards from './ProjectsCards.vue';
export default {
    components: {
        ProjectsCards
    },
    data() {
        return {
            projects: []
        }
    },
    mounted() {
        fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/projects')
            .then(response => response.json())
            .then(data => this.projects = data.list)
            .catch(error => console.error(error))
    }
}
</script>

<style></style>