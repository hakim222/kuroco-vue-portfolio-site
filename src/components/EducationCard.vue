<template>
  <div class="education-card" :data-order="education.order">
    <img :src="education.logo.url" :alt="education.subject + ' logo'" class="logo">
    <h3>{{ education.subject }}</h3>
    <p class="place">{{ education.place }}</p>
    <p class="qualification">{{ education.qualifications }}</p>
    <p class="description">{{ education.description }}</p>
  </div>
</template>

<script>
export default {
  name: 'EducationCard',
  props: {
    education: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.education-card {
  background-color: #2c2c2c;
  border-radius: 8px;
  padding: 1.5rem;
  margin-bottom: 1rem;
  cursor: move;
}

.logo {
  max-width: 100px;
  max-height: 100px;
  margin-bottom: 1rem;
}

h3 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.place, .qualification {
  font-size: 1rem;
  margin-bottom: 0.5rem;
}

.description {
  font-size: 0.9rem;
  line-height: 1.4;
}
</style>
