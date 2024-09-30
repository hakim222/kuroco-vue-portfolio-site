<template>
  <section id="about">
    <h2 class="section-title">About Me</h2>
    <div class="bento-grid">
      <div class="bento-item bio-card">
        <h3 class="name">Muhammad Hakim Bin 'Azizan</h3>
        <p class="location"><i class="fas fa-map-marker-alt"></i> Selangor, Malaysia</p>
      </div>
      
      <div class="bento-item bio-section" v-for="(section, index) in bioSections" :key="index">
        <h4 class="section-title">{{ section.title }}</h4>
        <p class="section-content">{{ section.content }}</p>
      </div>
      
      <div class="bento-item skills-container">
        <h4 class="section-title">My Skills</h4>
        <div v-if="skillsError" class="error-message">{{ skillsError }}</div>
        <div v-else class="skills-grid">
          <div v-for="skill in skills" :key="skill.topics_id" class="skill-item">
            <img :src="skill.image.url" :alt="skill.subject" class="skill-logo">
            <span class="skill-name">{{ skill.subject }}</span>
          </div>
        </div>
      </div>
      
      <div class="bento-item hobbies-container">
        <h4 class="section-title">Things I like~</h4>
        <div v-if="hobbiesError" class="error-message">{{ hobbiesError }}</div>
        <div v-else class="hobbies-grid">
          <div v-for="hobby in hobbies" :key="hobby.topics_id" class="hobby-item">
            <div class="hobby-image" :style="{ backgroundImage: `url(${hobby.image.url})` }">
              <span class="hobby-subject">{{ hobby.subject }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
    data() {
        return {
            bioSections: [
                {
                    title: "Background",
                    content: "Grown up in a moderate household, as any other 90's kids, 4 year's old me addicted to the classics, Wolfenstien 3D, original Doom as well as Terminal Velocity played on CRT monitored computers."
                },
                {
                    title: "Journey",
                    content: "Things escalated when my father bring home \"borrowed\" slightly modern computer from his workplace. Learnt to install games from the web and cd/disket locally. Discovered about Adventure Quest online game, enough to intrigued me about the science behind computer related technologies."
                },
                {
                    title: "Education & Career",
                    content: "Driven by young blooded curiosity, decided to pursue studies in IT related field. Graduated with the Bachelor Degree of Engineering, Computer Engineering in 2021, currently working as an Software Engineer and always inspired to be jack-of-all-trades in the IT industry!"
                }
            ],
            skills: [],
            hobbies: [],
            skillsError: null,
            hobbiesError: null
        }
    },

    mounted() {
        this.fetchHobbies()
        this.fetchSkills()
    },

    methods: {
        fetchHobbies() {
            fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/hobbies')
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then(data => this.hobbies = data.list)
                .catch(error => {
                    console.error('Error fetching hobbies:', error);
                    this.hobbiesError = 'Failed to load hobbies. Please try again later.';
                })
        },
        fetchSkills() {
            fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/skills')
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then(data => this.skills = data.list)
                .catch(error => {
                    console.error('Error fetching skills:', error);
                    this.skillsError = 'Failed to load skills. Please try again later.';
                })
        }
    }
}
</script>

<style scoped>
section {
  color: #e0e0e0;
  padding: 4rem 100px 0;
}

h2 {
    text-align: center;
    margin: 3rem 0;
    font-size: 2.5rem;
    font-weight: 300;
    letter-spacing: 2px;
}

.bento-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
}

.bento-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 20px;
  padding: 2rem;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

h3 {
  color: #ffffff;
  font-size: 1.8rem;
  margin-bottom: 1.5rem;
  position: relative;
  display: inline-block;
}

h3::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, #ff6b6b, #4ecdc4);
}

p {
  color: #cccccc;
  line-height: 1.6;
}

.skills-container, .hobbies-container {
  display: flex;
  flex-direction: column;
}

.skills-grid, .hobbies-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
  gap: 1.5rem;
}

.skill-item, .hobby-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.skill-logo {
  width: 60px;
  height: 60px;
  object-fit: contain;
  margin-bottom: 0.8rem;
}

.skill-name, .hobby-subject {
  font-size: 0.9rem;
  color: #ffffff;
  font-weight: 500;
}

.hobby-image {
  width: 100%;
  height: 120px;
  background-size: cover;
  background-position: center;
  border-radius: 10px;
  overflow: hidden;
  position: relative;
}

.hobby-subject {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.7);
  padding: 0.5rem;
}

.error-message {
  color: #ff6b6b;
  text-align: center;
  padding: 1rem;
  background: rgba(255, 107, 107, 0.1);
  border-radius: 10px;
  margin-top: 1rem;
}

@media (max-width: 768px) {
  .bento-grid {
    grid-template-columns: 1fr;
  }
  
  section {
    padding: 2rem 20px 0;
  }
}
</style>