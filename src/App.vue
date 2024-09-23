<template>
  <Navbar />
  <section id="top" style="height: 100vh; background-color: lightcoral;">Top Page</section>
  <section id="about" style="height: 100vh; background-color: lightseagreen;">About Me</section>
  <section id="experience" style="height: 100vh; background-color: lightblue;">Experience</section>
  <section id="education" style="height: 100vh; background-color: lightgoldenrodyellow;">Education</section>
  <Projects />
</template>

<script>
import Navbar from './components/Navbar.vue';
import Projects from './components/Projects.vue';

export default {
  name: 'App',
  components: {
    Navbar,
    Projects
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
