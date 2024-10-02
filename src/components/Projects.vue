<template>
    <section id="projects">
        <h2>Projects</h2>
        <div v-if="loading" class="loading">Loading projects...</div>
        <div v-else-if="error" class="error-message">
            {{ error }}
            <button @click="fetchProjects" class="retry-button">Retry</button>
        </div>
        <div v-else class="projects-grid">
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
            projects: [],
            loading: true,
            error: null
        }
    },
    mounted() {
        this.fetchProjects();
    },
    methods: {
        fetchProjects() {
            this.loading = true;
            this.error = null;
            fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/projects')
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Failed to fetch projects');
                    }
                    return response.json();
                })
                .then(data => {
                    this.projects = data.list;
                    this.loading = false;
                })
                .catch(error => {
                    console.error(error);
                    this.error = 'An error occurred while fetching projects. Please try again.';
                    this.loading = false;
                });
        }
    }
}
</script>

<style scoped>
section {
    color: #e0e0e0;
}

.projects-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 2rem;
    justify-content: center;
}

.loading, .error-message {
    text-align: center;
    font-size: 1.2rem;
    margin-top: 2rem;
}

.error-message {
    color: #ff6b6b;
}

.retry-button {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin-top: 10px;
    cursor: pointer;
    border-radius: 4px;
}

.retry-button:hover {
    background-color: #45a049;
}

@media (max-width: 1400px) {
    .projects-grid {
        grid-template-columns: repeat(3, 1fr);
    }
}

@media (max-width: 1100px) {
    .projects-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 768px) {
    section {
        padding: 4rem 3%;
    }
    h2 {
        font-size: 2rem;
    }
    .projects-grid {
        gap: 1.5rem;
    }
}

@media (max-width: 480px) {
    .projects-grid {
        grid-template-columns: 1fr;
    }
}
</style>