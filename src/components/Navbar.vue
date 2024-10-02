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

<script>
export default {
  data() {
    return {
      links: [
        { id: 'top', name: 'Home' },
        { id: 'about', name: 'About Me' },
        { id: 'experience', name: 'Experiences' },
        { id: 'education', name: 'Educations' },
        { id: 'projects', name: 'Projects' }
      ],
      activeSection: 'top',
      isScrolled: false
    }
  },
  methods: {
    scrollToSection(id) {
      const section = document.getElementById(id);
      const navbar = document.querySelector('.navbar');
      if (section && navbar) {
        let offset = -navbar.offsetHeight;
        if (id !== 'top') {
          offset = -navbar.offsetHeight + 10; // 3px offset
        }
        const sectionPosition = section.getBoundingClientRect().top + window.pageYOffset + offset;
        window.scrollTo({ top: sectionPosition, behavior: 'smooth' });
        this.activeSection = id;
      }
    },

    onScroll() {
      this.checkScroll();
      const sections = this.links.map(link => document.getElementById(link.id));
      const navbar = document.querySelector('.navbar');
      const navbarHeight = navbar ? navbar.clientHeight : 0;

      let activeSection = 'top';
      const scrollPosition = window.scrollY + navbarHeight - 3; // 3px offset

      for (let i = 1; i < sections.length; i++) {
        const section = sections[i];
        if (section) {
          const sectionTop = section.offsetTop;
          if (scrollPosition >= sectionTop) {
            activeSection = section.id;
          } else {
            break;
          }
        }
      }

      this.activeSection = activeSection;
    },

    checkScroll() {
      this.isScrolled = window.scrollY > 0;
    }
  },
  mounted() {
    window.addEventListener('scroll', this.onScroll);
    this.checkScroll();
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.onScroll);
  }
}
</script>

<style scoped>
.navbar {
  display: flex;
  align-items: center;
  position: sticky;
  top: 0;
  width: 100%;
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
  margin: 0 100px;
}

.brand-name {
  font-family: 'Orbitron', sans-serif;
  font-size: 2rem;
  font-weight: bold;
  background: linear-gradient(45deg, #4ecdc4, #45b7aa, #3a9791);
  background-size: 200% 200%;
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  animation: gradientShift 8s ease infinite;
}

@keyframes gradientShift {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.nav-links {
  display: flex;
  justify-content: center;
  padding: 1rem 1rem;
  border-radius: 1.5rem;
  list-style-type: none;
  margin: 1rem 0;
  gap: 1rem;
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
  border-color: #4ecdc4;
}

li {
  display: flex;
}

@media (max-width: 1246px) {
  .brand-name {
    font-size: 1.3rem;
  }
}

@media (max-width: 1024px) {
  .nav-container {
    margin: 0 0;
    justify-content: center;
  }
  .brand-name {
    display: none;
  }
}

@media (max-width: 690px) {
  a {
    font-size: 0.8rem;
  }
  .nav-links {
    flex-wrap: wrap;
    padding: 0.5rem 0.5rem;
  }

  li {
    display: flex;
  }
}

@media (max-width: 612px) {
  .nav-links {
    margin-left: 10px;
    margin-right: 10px;
  }
}

@media (max-width: 480px) {
  .nav-links {
    padding: 0.5rem 0.5rem;
  }
  .nav-links a {
    font-size: 0.7rem;
    padding: 0.2rem 0.5rem;
  }
  
}

</style>