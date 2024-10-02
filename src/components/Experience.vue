<template>
    <section id="experience">
        <h2>Work Experiences</h2>
        <div v-if="experiencesError" class="error-message">{{ experiencesError }}</div>
        <div v-else class="timeline">
            <div v-for="(exp, index) in experiences" :key="exp.topics_id" class="timeline-item" :class="{ 'left': index % 2 === 0, 'right': index % 2 !== 0 }">
                <div class="timeline-content">
                    <p class="date">{{ exp.date_duration }}</p>
                    <h3>{{ exp.subject }}</h3>
                    <p class="company"><i class="fas fa-map-marker-alt"></i> {{ exp.company }}</p>
                    <p class="description">{{ exp.description }}</p>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    data() {
        return {
            experiences: [],
            experiencesError: null
        }
    },

    mounted() {
        this.fetchExperiences()
    },

    methods: {
        fetchExperiences() {
            fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/experiences')
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then(data => this.experiences = data.list)
                .catch(error => {
                    console.error('Error fetching experiences:', error);
                    this.experiencesError = 'Failed to load work experiences. Please try again later.';
                })
        }
    }
}
</script>

<style scoped>
section {
    color: #e0e0e0;
}

.timeline {
    position: relative;
    margin: 0 auto;
    padding: 0 20px;
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

.timeline-item.left {
    left: 0;
}

.timeline-item.right {
    left: 50%;
}

.timeline-content {
    padding: 20px;
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

.timeline-item::after {
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

.timeline-item.left::after {
    right: -10px;
}

.timeline-item.right::after {
    left: -10px;
}

.date {
    font-size: 0.9rem;
    color: #757575;
    margin-bottom: 0.5rem;
}

h3 {
    margin-top: 0;
    color: #ffffff;
    font-size: 1.5rem;
    font-weight: 500;
    margin-bottom: 0.5rem;
}

.company {
    font-weight: 300;
    color: #bdbdbd;
}

.description {
    margin-top: 1rem;
    font-size: 0.95rem;
    color: #d0d0d0;
    line-height: 1.5;
}

.error-message {
    color: #ff6b6b;
    text-align: center;
    padding: 1rem;
    background: rgba(255, 107, 107, 0.1);
    border-radius: 10px;
    margin-top: 1rem;
}

@media (max-width: 600px) {
    .timeline::after {
        left: 31px;
    }

    .timeline-item {
        width: 100%;
        padding-left: 70px;
        padding-right: 25px;
    }

    .timeline-item.right {
        left: 0;
    }

    .timeline-item.left::after,
    .timeline-item.right::after {
        left: 21px;
    }
}

@media (max-width: 480px) {
  .timeline {
    padding: 0;
  }
  .timeline::after {
    left: 20px;
    width: 5px;
  }

  .timeline-item.left::after,
  .timeline-item.right::after {
    left: 10px;
  }

  .timeline-item {
    padding-left: 2.5rem;
    padding-right: 10px;
  }

  .timeline-content {
    padding: 1rem;
  }

  h3,.date, .company, .description {
    margin: 0;
  }

  .date {
    font-size: 0.8rem;
    margin-bottom: 0.5rem;
  }

  h3 {
    font-size: 1rem;
    margin-bottom: 0.5rem;
    font-weight: 400;
  }

  .company {
    font-size: 0.8rem;
    margin-bottom: 0.5rem;
  }

  .description {
    font-size: 0.8rem;
  }
}
</style>