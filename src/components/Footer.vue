<template>
    <footer class="footer">
        <div class="footer-content">
            <p>&copy; {{ currentYear }} Hakim Azizan. All rights reserved.</p>
            <div class="social-links">
                <a v-for="social in socials" :key="social.name" :href="social.url" target="_blank" rel="noopener noreferrer">
                    <i :class="social.icon"></i>
                </a>
            </div>
        </div>
    </footer>
</template>

<script>
export default {
    data() {
        return {
            currentYear: new Date().getFullYear(),
            socials: [
                { name: 'LinkedIn', icon: 'fab fa-linkedin', url: 'https://linkedin.com/in/hakim-azizan-65878b145' },
                { name: 'Twitter', icon: 'fab fa-twitter', url: 'https://twitter.com/hakimazizan98' },
                { name: 'Instagram', icon: 'fab fa-instagram', url: 'https://instagram.com/hakim_azizan' },
                { name: 'Facebook', icon: 'fab fa-facebook', url: 'https://facebook.com/hakim.powerhot' },
                { name: 'GitHub', icon: 'fab fa-github', url: 'https://github.com/hakim222' },
            ],
        }
    }
}
</script>

<style scoped>
.footer {
    margin-top: 50px;
    background-color: #2d2d2d;
    color: #fff;
    padding: 20px 0;
    text-align: center;
  border-top: 1px solid rgba(255, 255, 255, 0.3);
}

.footer-content {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.social-links a {
    color: #fff;
    margin: 0 10px;
    text-decoration: none;
    font-size: 1.2rem;
}

.social-links a:hover {
    color: #4ecdc4;
}
</style>
