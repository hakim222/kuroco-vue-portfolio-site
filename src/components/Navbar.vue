<template>
    <nav class="navbar">
      <ul class="nav-links">
        <li v-for="link in links" :key="link.id">
          <a href="#" @click.prevent="scrollToSection(link.id)">{{ link.name }}</a>
        </li>
      </ul>
    </nav>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  
  const links = ref([
    { id: 'top', name: 'Top Page' },
    { id: 'about', name: 'About Me' },
    { id: 'experience', name: 'Experience' },
    { id: 'education', name: 'Education' },
    { id: 'projects', name: 'Projects' }
  ])
  
  const scrollToSection = (id) => {
    const section = document.getElementById(id)
    if (section) {
      section.scrollIntoView({ behavior: 'smooth' })
    }
  }
  </script>
  
  <style scoped>
  .navbar {
    position: fixed;
    top: 0;
    width: 100%;
    background-color: white;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    z-index: 1000;
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
    transition: color 0.3s;
  }
  
  .nav-links a:hover {
    color: #007bff;
  }
  </style>
  