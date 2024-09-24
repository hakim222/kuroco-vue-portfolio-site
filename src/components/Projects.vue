<template>
    <section id="projects">
        <h2>Projects</h2>
        <div class="projects-grid">
            <ProjectsCards v-for="project in projects" :key="project.topics_id" :project="project" />
        </div>
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

<style scoped>
section {
    padding: 4rem 100px;
    color: #e0e0e0;
}
h2 {
    text-align: center;
    margin-bottom: 3rem;
    font-size: 2.5rem;
    font-weight: 300;
    letter-spacing: 2px;
}
.projects-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 2rem;
    row-gap: 0rem;
}
</style>