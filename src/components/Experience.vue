<template>
    <section id="experience">
        <h2>Work Experiences</h2>
        <div class="timeline">
            <div v-for="exp in experiences" :key="exp.topics_id" class="timeline-item">
                <div class="timeline-content">
                    <h3>{{ exp.subject }}</h3>
                    <p class="company">{{ exp.company }}</p>
                    <p class="date">{{ exp.date_duration }}</p>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    data() {
        return {
            experiences: []
        }
    },

    mounted() {
        fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/experiences')
            .then(response => response.json())
            .then(data => this.experiences = data.list)
            .catch(error => console.error(error))
    }
}
</script>

<style scoped>
section {
    height: 100vh;
    color: #e0e0e0;
    padding: 4rem 100px 0px;
}

h2 {
    text-align: center;
    margin-bottom: 3rem;
    font-size: 2.5rem;
    font-weight: 300;
    letter-spacing: 2px;
}

.timeline {
    position: relative;
    max-width: 1200px;
    margin: 0 auto;
}

.timeline::after {
    content: '';
    position: absolute;
    width: 2px;
    background-color: rgba(255, 255, 255, 0.1);
    top: 0;
    bottom: 0;
    left: 50%;
    margin-left: -1px;
}

.timeline-item {
    padding: 10px 40px;
    position: relative;
    width: 50%;
    box-sizing: border-box;
}

.timeline-item:nth-child(odd) {
    left: 0;
    padding-right: 50px;
}

.timeline-item:nth-child(even) {
    left: 50%;
    padding-left: 50px;
}

.timeline-content {
    padding: 20px 30px;
    background-color: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(10px);
    position: relative;
    border-radius: 10px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.1);
    transition: all 0.3s ease;
}

.timeline-content:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.2);
}

.timeline-content::after {
    content: '';
    position: absolute;
    width: 20px;
    height: 20px;
    background-color: #333;
    border: 2px solid rgba(255, 255, 255, 0.3);
    top: 15px;
    border-radius: 50%;
    z-index: 1;
}

.timeline-item:nth-child(odd) .timeline-content::after {
    right: -60px;
}

.timeline-item:nth-child(even) .timeline-content::after {
    left: -60px;
}

h3 {
    margin-top: 0;
    color: #ffffff;
    font-size: 1.5rem;
    font-weight: 500;
}

.company {
    font-weight: 300;
    color: #bdbdbd;
    margin-bottom: 0.5rem;
}

.date {
    font-size: 0.9rem;
    color: #757575;
}

</style>