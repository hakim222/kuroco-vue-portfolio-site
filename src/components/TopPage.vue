<template>
  <section id="top">
    <div class="container">
      <div class="background-animation"></div>
      <div class="content">
        <p>Hello there! Welcome to my site.</p>
        <h1 class="name">Hakim 'Azizan</h1>
        <p class="title">Front-end Developer & UI/UX Enthusiast</p>
        <p class="description">
          Crafting elegant solutions with code and design. Specializing in web technologies and creating seamless user
          experiences.
        </p>
        <div class="social-links">
          <a v-for="social in socials" :key="social.name" :href="social.url" target="_blank" rel="noopener noreferrer">
            <i :class="social.icon"></i>
          </a>
        </div>
      </div>
      <div class="image-container">
        <img src="/img/potrait_top_page.jpg" alt="Hakim 'Azizan portrait" />
      </div>
    </div>

    <div class="scroll-indicator">
      <span>Scroll Down</span>
      <i class="fas fa-chevron-down"></i>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      socials: [
        { name: 'LinkedIn', icon: 'fab fa-linkedin', url: 'https://linkedin.com/in/hakim-azizan-65878b145' },
        { name: 'Twitter', icon: 'fab fa-twitter', url: 'https://twitter.com/hakimazizan98' },
        { name: 'Instagram', icon: 'fab fa-instagram', url: 'https://instagram.com/hakim_azizan' },
        { name: 'Facebook', icon: 'fab fa-facebook', url: 'https://facebook.com/hakim.powerhot' },
        { name: 'GitHub', icon: 'fab fa-github', url: 'https://github.com/hakim222' },
      ],
    };
  },
}
</script>

<style scoped>
section {
  padding: 100px;
  color: #fafafa;
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  position: relative;
  overflow: hidden;
}

.container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  max-width: 50%;
}

h1 {
  font-size: 3.5rem;
  font-weight: 700;
  margin: 0 0 1rem;
}

.title {
  font-size: 2rem;
  font-weight: 600;
  margin: 0 0 1rem;
  color: #4ecdc4;
}

.description {
  font-size: 1.2rem;
  line-height: 1.6;
}

p {
  font-size: 1.8rem;
  margin: 0 0 2rem;
  color: #a0a0a0;
}

.social-links {
  display: flex;
  gap: 1rem;
}

.social-links a {
  color: #fafafa;
  font-size: 1.5rem;
  transition: color 0.3s ease;
}

.social-links a:hover {
  color: #4ecdc4;
}

.image-container {
  width: 40%;
  height: 70vh;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  border: 1px solid rgba(255, 255, 255, 0.18);
}

.image-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.name {
  font-size: 4rem;
  background: linear-gradient(45deg, #4ecdc4 0%, #ff6b6b 25%, #4ecdc4 50%, #ff6b6b 75%, #4ecdc4 100%);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  background-size: 400% 100%;
  animation: gradient-shift 12s linear infinite;
}

@keyframes gradient-shift {
  0% {
    background-position: 0% 0%;
  }

  100% {
    background-position: 400% 0%;
  }
}

.background-animation {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(45deg, rgba(78, 205, 196, 0.1) 25%, transparent 25%),
    linear-gradient(-45deg, rgba(255, 107, 107, 0.1) 25%, transparent 25%);
  background-size: 60px 60px;
  animation: moveBackground 10s linear infinite;
  z-index: -1;
}

@keyframes moveBackground {
  0% {
    background-position: 0 0;
  }

  100% {
    background-position: 60px 60px;
  }
}

.scroll-indicator {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #a0a0a0;
  font-size: 0.9rem;
}

.scroll-indicator i {
  margin-top: 5px;
  animation: bounce 2s infinite;
}

@keyframes bounce {

  0%,
  20%,
  50%,
  80%,
  100% {
    transform: translateY(0);
  }

  40% {
    transform: translateY(-10px);
  }

  60% {
    transform: translateY(-5px);
  }
}

@media (max-width: 768px) {

  p {
    font-size: 1rem;
  }

  .name {
    font-size: 2rem;
  }

  .title {
    font-size: 1.5rem;
  }

  .image-container {
    height: 400px;
  }


}

@media (max-width: 480px) {
  .container {
    flex-direction: column;
    row-gap: 1rem;
  }

  .content {
    max-width: 100%;
  }

  p {
    margin-bottom: 1rem;
  }

  .image-container {
    width: 100%;
    height: 350px;
  }
}
</style>