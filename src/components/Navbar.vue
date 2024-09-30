<template>
  <nav :class="['navbar', { 'navbar-scrolled': isScrolled }]">
    <div class="nav-container">
      <h1 class="brand-name">Hakim 'Azizan</h1>
      <ul class="nav-links">
        <li v-for="link in links" :key="link.id">
          <a :href="'#' + link.id" @click.prevent="scrollToSection(link.id)"
            :class="{ active: activeSection === link.id }">{{ link.name }}</a>
        </li>
      </ul>
    </div>

  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const links = ref([
  { id: 'top', name: 'Home' },
  { id: 'about', name: 'About Me' },
  { id: 'experience', name: 'Experiences' },
  { id: 'education', name: 'Educations' },
  { id: 'projects', name: 'Projects' }
])

const activeSection = ref('top')

const scrollToSection = (id) => {
  const section = document.getElementById(id);
  const navbar = document.querySelector('.navbar');
  if (section && navbar) {
    let offset = 0;
    if (id === 'top') {
      offset = -navbar.offsetHeight;
    }
    const sectionPosition = section.getBoundingClientRect().top + window.pageYOffset + offset;
    window.scrollTo({ top: sectionPosition, behavior: 'smooth' });
    activeSection.value = id;
  }
}

const onScroll = () => {
  checkScroll()
  const sections = links.value.map(link => document.getElementById(link.id));
  const navbar = document.querySelector('.navbar');
  const offset = navbar ? navbar.clientHeight : 0;

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

const isScrolled = ref(false)

const checkScroll = () => {
  isScrolled.value = window.scrollY > 0
}

onMounted(() => {
  window.addEventListener('scroll', onScroll);
  checkScroll()
});

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll);
});
</script>

<style scoped>
.navbar {
  display: flex;
  align-items: center;
  padding: 0px;
  position: sticky;
  top: 0;
  width: 100%;
  height: auto;
  z-index: 1000;
  transition: all 0.3s ease;
}

.navbar-scrolled {
  backdrop-filter: blur(19px) saturate(5%);
  -webkit-backdrop-filter: blur(19px) saturate(5%);
  background-color: rgba(0, 0, 0, 0.55);
  border-bottom: 1px solid rgba(209, 213, 219, 0.3);
}

.nav-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  margin: 0px 100px;
}

.brand-name {
  font-family: 'Orbitron', sans-serif;
  font-size: 2.5rem;
  font-weight: bold;
  background: linear-gradient(45deg, #4ecdc4, #45b7aa, #3a9791);
  background-size: 200% 200%;
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  animation: gradientShift 8s ease infinite;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  letter-spacing: 1px;
  transition: transform 0.3s ease;
}

.brand-name:hover {
  transform: scale(1.05);
}

@keyframes gradientShift {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.nav-links {
  display: flex;
  justify-content: center;
  padding: 1.2rem 1rem;
  border-radius: 1.5rem;
  list-style-type: none;
  margin: 0;
  gap: 1rem;
  margin: 1rem 0;
  background: rgba(54, 54, 54, 0.733);
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.nav-links a {
  text-decoration: none;
  background-color: #363636;
  border: 2px solid #ababab;
  color: #ababab;
  font-weight: bold;
  transition: color 0.3s, background-color 0.3s, border-radius 0.3s, padding 0.3s, border-color 0.3s;
  padding: 0.5rem 1rem;
  border-radius: 1rem;
}

.nav-links a:hover {
  color: #f0f0f0;
  border-color: #f0f0f0;
}

.nav-links a.active {
  background-color: #3a9791;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 1rem;
  border-color: #4ecdc4;
}
</style>