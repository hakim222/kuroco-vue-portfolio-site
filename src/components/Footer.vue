<template>
    <footer class="footer">
        <div class="footer-content">
            <p>&copy; {{ currentYear }} Hakim Azizan. All rights reserved.</p>
            <div class="social-links">
                <a href="https://github.com/yourusername" target="_blank" rel="noopener noreferrer">GitHub</a>
                <a href="https://linkedin.com/in/yourusername" target="_blank" rel="noopener noreferrer">LinkedIn</a>
                <a href="mailto:your.email@example.com">Email</a>
            </div>
        </div>
    </footer>
</template>

<script>
export default {
    data() {
        return {
            currentYear: new Date().getFullYear()
        }
    }
}
</script>

<style scoped>
.footer {
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
}

.social-links a:hover {
    text-decoration: underline;
}
</style>
