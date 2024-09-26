<template>
    <section id="about">
        <h2>About Me</h2>
        <div class="bento-grid">
            <div class="bento-item bio">
                <h3>Muhammad Hakim Bin 'Azizan</h3>
                <p>Selangor, Malaysia</p>
                <p>
                    Grown up in a moderate household, as any other 90's kids, 4 year's old me addicted to the classics,
                    Wolfenstien 3D, original Doom as well as Terminal Velocity played on CRT monitored computers.
                </p>
                <p>
                    Things escalated when my father bring home "borrowed" slightly modern computer from his workplace.
                    Learnt to install games from the web and cd/disket locally. Discovered about Adventure Quest online
                    game, enough to intrigued me about the science behind computer related technologies.
                </p>
                <p>
                    Driven by young blooded curiosity, decided to pursue studies in IT related field. Graduated with the
                    Bachelor Degree of Engineering, Computer Engineering in 2021, currently working as an IT Engineer
                    and always inspired to be jack-of-all-trades in the IT industry!
                </p>
            </div>
            <div class="bento-item hobbies-container">
                <h3 class="hobbies-title">Things I like~</h3>
                <div class="hobbies-content">
                    <div class="hobbies-grid">
                        <div v-for="hobby in hobbies" :key="hobby.topics_id" class="hobby-item">
                            <div class="hobby-image" :style="{ backgroundImage: `url(${hobby.image.url})` }">
                                <span class="hobby-subject">{{ hobby.subject }}</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    data() {
        return {
            hobbies: []
        }
    },

    mounted() {
        fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/hobbies')
            .then(response => response.json())
            .then(data => this.hobbies = data.list)
            .catch(error => console.error(error))
    }
}
</script>

<style scoped>
section {
    color: #e0e0e0;
    padding: 4rem 100px 0px;
}
h2 {
    text-align: center;
    margin: 3rem 0rem;
    font-size: 2.5rem;
    font-weight: 300;
    letter-spacing: 2px;
}
.bento-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
}

.bento-item {
    background: rgba(255, 255, 255, 0.1);
    border-radius: 10px;
    padding: 1rem;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.1);
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.bio {
    grid-column: 1 / -1;
}

.hobbies-container {
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    gap: 2rem;
}

.hobbies-title {
    font-size: 1.5rem;
    white-space: nowrap;
    margin-right: 1rem;
}

.hobbies-content {
    flex-grow: 1;
}

.hobbies-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(2, 1fr);
    gap: 1rem;
    width: 100%;
}

.hobby-item {
    aspect-ratio: 1 / 1;
    overflow: hidden;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.hobby-image {
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    display: flex;
    align-items: flex-end;
    justify-content: center;
    position: relative;
}

.hobby-subject {
    background-color: rgba(0, 0, 0, 0.7);
    color: #ffffff;
    padding: 0.5rem;
    font-size: 0.9rem;
    width: 100%;
    text-align: center;
}

h2, h3 {
    color: #ffffff;
}

p {
    color: #cccccc;
}
</style>