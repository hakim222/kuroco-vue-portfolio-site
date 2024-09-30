<template>
  <div class="education-card" :data-order="education.order">
    <div class="logo-container">
      <img :src="education.logo.url" :alt="education.subject + ' logo'" class="logo">
    </div>
    <div class="card-content">
      <h3>{{ education.subject }}</h3>
      <p class="year"><i class="fas fa-calendar-alt"></i> {{ education.year }}</p>
      <p class="place"><i class="fas fa-map-marker-alt"></i>   {{ education.place }}</p>
      <p class="qualification">{{ education.qualifications }}</p>
      <p class="description">{{ education.description }}</p>
    </div>
    <div class="drag-handle">
      <i class="fas fa-grip-lines"></i>
    </div>
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
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  overflow: hidden;
  transition: all 0.3s ease;
  position: relative;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  flex: 0 0 auto;
  width: 400px; /* Increased from 300px */
  min-height: 500px; /* Increased from 400px */
  margin-right: 1.5rem;
  scroll-snap-align: start;
  display: flex;
  flex-direction: column;
  border: 1px solid rgba(255, 255, 255, 0.18);
  cursor: move;
}

.education-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2);
}

.logo-container {
  height: 140px; /* Increased from 120px */
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(255, 255, 255, 0.1);
  padding: 1rem;
}

.logo {
  max-width: 80%;
  max-height: 80%;
  object-fit: contain;
}

.card-content {
  flex-grow: 1;
  padding: 1.5rem; /* Increased from 1rem */
  overflow-y: auto;
}

h3 {
  font-size: 1.5rem; /* Increased from 1.2rem */
  margin-bottom: 0.75rem; /* Increased from 0.5rem */
  color: #ffffff;
}

.place, .qualification {
  font-size: 1.1rem; /* Increased from 0.9rem */
  margin-bottom: 0.5rem; /* Increased from 0.3rem */
  color: rgba(255, 255, 255, 0.8);
}

.description {
  font-size: 1rem; /* Increased from 0.8rem */
  line-height: 1.5; /* Increased from 1.4 */
  color: rgba(255, 255, 255, 0.7);
}

.drag-handle {
  position: absolute;
  top: 0;
  right: 0;
  padding: 0.5rem;
  cursor: move;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.education-card:hover .drag-handle {
  opacity: 1;
}

.drag-handle i {
  color: rgba(255, 255, 255, 0.7);
}

.year {
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  color: rgba(255, 255, 255, 0.8);
}

/* Scrollbar styles for webkit browsers */
.card-content::-webkit-scrollbar {
  width: 6px;
}

.card-content::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
}

.card-content::-webkit-scrollbar-thumb {
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 20px;
}
</style>
