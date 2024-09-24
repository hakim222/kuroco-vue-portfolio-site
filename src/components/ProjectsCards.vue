<template>
    <div class="project-card glassmorphism">
        <div>
            <img v-if="project.image && project.image.url" :src="project.image.url" :alt="project.subject"
                class="project-image">
            <h2 class="project-title">{{ project.subject }}</h2>
            <p class="project-description">{{ project.description }}</p>
        </div>

        <div class="vote-section">
            <p class="vote-count">Votes: {{ project.votes }}</p>
            <div class="button-group">
                <button v-if="!hasVoted" @click="upvote" class="vote-button">
                    <i class="fas fa-thumbs-up"></i> Upvote
                </button>
                <button v-if="!hasVoted" @click="downvote" class="vote-button downvote">
                    <i class="fas fa-thumbs-down"></i> Downvote
                </button>
                <p v-else :class="['vote-message', voteType]">{{ voteMessage }}</p>
            </div>
        </div>

    </div>
</template>

<script>
export default {
    props: ['project'],
    data() {
        return {
            hasVoted: false,
            voteMessage: '',
            voteType: '',
        }
    },
    mounted() {
        this.checkVoteStatus()
    },
    methods: {
        checkVoteStatus() {
            const voteStatus = localStorage.getItem(`vote_${this.project.topics_id}`)
            if (voteStatus) {
                this.hasVoted = true
                this.voteType = voteStatus
                this.voteMessage = voteStatus === 'upvote' ? 'You have Upvoted this project' : 'You have Downvoted this project'
            }
        },
        async upvote() {
            try {
                const response = await fetch(`https://hakim-azizan.g.kuroco.app/rcms-api/1/upvoteproject/${this.project.topics_id}`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({})
                })
                if (!response.ok) {
                    throw new Error('Failed to upvote project')
                }
                this.project.votes++
                this.hasVoted = true
                this.voteMessage = 'You have Upvoted this project'
                this.voteType = 'upvote'
                localStorage.setItem(`vote_${this.project.topics_id}`, 'upvote')
            } catch (error) {
                console.error(error)
            }
        },
        async downvote() {
            try {
                const response = await fetch(`https://hakim-azizan.g.kuroco.app/rcms-api/1/downvoteproject/${this.project.topics_id}`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({})
                })
                if (!response.ok) {
                    throw new Error('Failed to downvote project')
                }
                this.project.votes--
                this.hasVoted = true
                this.voteMessage = 'You have Downvoted this project'
                this.voteType = 'downvote'
                localStorage.setItem(`vote_${this.project.topics_id}`, 'downvote')
            } catch (error) {
                console.error(error)
            }
        }
    }
}
</script>

<style scoped>
.project-card {
    background-color: rgba(40, 40, 40, 0.7);
    border-radius: 15px;
    padding: 25px;
    margin: 20px;
    max-width: 350px;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    color: #e0e0e0;
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.18);
    transition: all 0.3s ease;
}

.project-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 40px 0 rgba(0, 0, 0, 0.5);
}

.project-title {
    font-size: 1.5rem;
    margin-bottom: 15px;
    color: #ffffff;
}

.project-description {
    font-size: 1rem;
    margin-bottom: 20px;
    line-height: 1.5;
}

.vote-count {
    font-size: 1.1rem;
    font-weight: bold;
    margin-bottom: 15px;
}

.button-group {
    display: flex;
    gap: 10px;
}

.vote-button {
    background-color: rgba(60, 60, 60, 0.6);
    color: #ffffff;
    border: none;
    padding: 10px 20px;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    font-weight: bold;
}

.vote-button:hover {
    background-color: rgba(80, 80, 80, 0.8);
}

.vote-button.downvote {
    background-color: rgba(180, 60, 60, 0.6);
}

.vote-button.downvote:hover {
    background-color: rgba(200, 80, 80, 0.8);
}

.project-image {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 10px;
    margin-bottom: 15px;
}

.vote-message {
    font-size: 0.9rem;
    font-weight: bold;
    color: #ffffff;
    text-align: center;
    padding: 8px 15px;
    border-radius: 20px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    margin: 0px;
}

.vote-message::before {
    font-family: 'Font Awesome 5 Free';
    margin-right: 5px;
}

.vote-message.upvote {
    background-color: rgba(0, 44, 0, 0.6);
    border: 1px solid rgba(144, 238, 144, 0.4);
    color:rgb(97, 160, 97);
}

.vote-message.upvote::before {
    content: '\f164';
}

.vote-message.downvote {
    background-color: rgba(78, 0, 0, 0.6);
    border: 1px solid rgba(255, 99, 71, 0.4);
    color:rgb(180, 74, 55);
}

.vote-message.downvote::before {
    content: '\f165';
}

.vote-message {
    font-size: 1rem;
    font-weight: bold;
    color: #ffffff;
    text-align: center;
}
</style>