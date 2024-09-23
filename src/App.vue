<template>
  <Navbar />
  <TopPage />
  <AboutMe />
  <Experience />
  <Education />
  <Projects />
</template>

<script>
import Navbar from './components/Navbar.vue';
import TopPage from './components/TopPage.vue';
import AboutMe from './components/AboutMe.vue';
import Experience from './components/Experience.vue';
import Education from './components/Education.vue';
import Projects from './components/Projects.vue';

export default {
  name: 'App',
  components: {
    Navbar,
    TopPage,
    AboutMe,
    Experience,
    Education,
    Projects
  }
}
</script>

<style>
</style>
