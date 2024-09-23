<template>
  <nav class="navbar">
    <ul class="nav-links">
      <li v-for="link in links" :key="link.id">
        <a :href="'#' + link.id" @click.prevent="scrollToSection(link.id)" :class="{ active: activeSection === link.id }">{{ link.name }}</a>
      </li>
    </ul>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const links = ref([
  { id: 'top', name: 'Home' },
  { id: 'about', name: 'About Me' },
  { id: 'experience', name: 'Experience' },
  { id: 'education', name: 'Education' },
  { id: 'projects', name: 'Projects' }
])

const activeSection = ref('top')

const scrollToSection = (id) => {
  const section = document.getElementById(id);
  const navbar = document.querySelector('.navbar'); 
  if (section && navbar) {
    const offset = -navbar.offsetHeight;
    const sectionPosition = section.getBoundingClientRect().top + window.pageYOffset + offset;
    window.scrollTo({ top: sectionPosition, behavior: 'smooth' });
    activeSection.value = id;
  }
}

const onScroll = () => {
  const sections = links.value.map(link => document.getElementById(link.id));
  const navbar = document.querySelector('.navbar');
  const offset = navbar ? navbar.offsetHeight : 0;

  sections.forEach(section => {
    if (section) {
      const sectionTop = section.getBoundingClientRect().top - offset;
      const sectionBottom = sectionTop + section.offsetHeight;
      if (sectionTop <= 0 && sectionBottom > 0) {
        activeSection.value = section.id;
      }
    }
  });
}

onMounted(() => {
  window.addEventListener('scroll', onScroll);
});

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll);
});
</script>

<style scoped>
.navbar {
  padding: 0px 0px;
  position: sticky;
  align-items: center;
  top: 0;
  width: 100%;
  background-color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.nav-links {
  display: flex;
  justify-content: center;
  padding: 1rem 0;
  list-style-type: none;
  margin: 0;
}

.nav-links li {
  margin: 0 1rem;
}

.nav-links a {
  text-decoration: none;
  color: #333;
  font-weight: bold;
  transition: color 0.3s, background-color 0.3s, border-radius 0.3s, padding 0.3s;
  padding: 0.5rem 1rem;
  border-radius: 0.25rem; 
}

.nav-links a:hover {
  color: #007bff;
}

.nav-links a.active {
  background-color: #007bff;
  color: white;
  border-radius: 0.5rem;
  padding: 0.75rem 1.25rem;
}
</style>