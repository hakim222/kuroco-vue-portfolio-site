<template>
    <div class="project-card glassmorphism">
        <div>
            <img v-if="project.image && project.image.url" :src="project.image.url" :alt="project.subject"
                class="project-image">
            <div class="project-header">
                <h2 class="project-title">{{ project.subject }}</h2>
                <div class="project-links">
                    <a v-if="project.github" :href="project.github" target="_blank" rel="noopener noreferrer" class="icon-link github-link">
                        <i class="fab fa-github"></i>
                    </a>
                    <a v-if="project.app_url" :href="project.app_url" target="_blank" rel="noopener noreferrer" class="icon-link app-link">
                        <i class="fas fa-external-link-alt"></i>
                    </a>
                </div>
            </div>
            <p class="project-description">{{ project.description }}</p>
            <div class="tech-tags">
                <span v-for="tag in techTags" :key="tag" class="tech-tag">{{ tag }}</span>
            </div>
        </div>

        <div class="vote-section">
            <p class="vote-count">Votes: {{ project.votes }}</p>
            <div class="button-group">
                <template v-if="!hasVoted && !isVoting && !showError">
                    <button @click="upvote" class="vote-button">
                        <i class="fas fa-thumbs-up"></i> Upvote
                    </button>
                    <button @click="downvote" class="vote-button downvote">
                        <i class="fas fa-thumbs-down"></i> Downvote
                    </button>
                </template>
                <p v-if="isVoting" class="vote-message voting">{{ votingMessage }}</p>
                <p v-if="hasVoted && !isVoting" :class="['vote-message', voteType]">{{ voteMessage }}</p>
                <p v-if="showError" class="vote-message error">{{ errorMessage }}</p>
            </div>
        </div>

    </div>
</template>

<script>
export default {
    props: {
        project: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            hasVoted: false,
            voteMessage: '',
            voteType: '',
            isVoting: false,
            votingMessage: '',
            showError: false,
            errorMessage: '',
        }
    },
    computed: {
        voteEndpoint() {
            return `https://hakim-azizan.g.kuroco.app/rcms-api/1/${this.voteType}project/${this.project.topics_id}`
        },
        techTags() {
            return this.project.tech_tags ? this.project.tech_tags.split(',').map(tag => tag.trim()) : []
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
        vote(type) {
            this.voteType = type;
            this.isVoting = true;
            this.votingMessage = `${type === 'upvote' ? 'Upvoting' : 'Downvoting'}...`;

            fetch(this.voteEndpoint, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({})
            })
            .then(response => {
                if (!response.ok) {
                    throw new Error(`Failed to ${type} project`);
                }
                return new Promise(resolve => setTimeout(resolve, 1000)); 
            })
            .then(() => {
                this.project.votes += type === 'upvote' ? 1 : -1;
                this.hasVoted = true;
                this.voteMessage = `You have ${type === 'upvote' ? 'upvoted' : 'downvoted'} this project`;
                localStorage.setItem(`vote_${this.project.topics_id}`, type);
            })
            .catch(error => {
                console.error(error);
                this.showError = true;
                this.errorMessage = `Failed to ${type} project`;
                setTimeout(() => {
                    this.showError = false;
                    this.errorMessage = '';
                }, 2000);
            })
            .finally(() => {
                this.isVoting = false;
            });
        },
        upvote() {
            this.vote('upvote')
        },
        downvote() {
            this.vote('downvote')
        }
    }
}
</script>

<style scoped>
.project-card {
    background-color: rgba(40, 40, 40, 0.7);
    border-radius: 15px;
    padding: 25px;
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
    text-align: left;
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

.vote-message.voting {
    background-color: rgba(60, 60, 60, 0.6);
    border: 1px solid rgba(255, 255, 255, 0.4);
    color: #ffffff;
}

.vote-message.voting::before {
    content: '\f110';
    animation: fa-spin 2s infinite linear;
}

@keyframes fa-spin {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}

.project-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 15px;
}

.project-links {
    display: flex;
    gap: 10px;
}

.icon-link {
    font-size: 1.5rem;
    color: #ffffff;
    transition: color 0.3s ease;
}

.github-link:hover {
    color: #45b7aa;
}

.app-link:hover {
    color: #45b7aa;
}

.tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-top: 15px;
    margin-bottom: 20px;
}

.tech-tag {
    background-color: rgba(69, 183, 170, 0.2);
    color: #45b7aa;
    padding: 4px 10px;
    border-radius: 20px;
    font-size: 0.8rem;
    font-weight: bold;
}

@media (max-width: 480px) {
    .project-card {
        padding: 20px;
    }

    .project-title {
        font-size: 1.3rem;
    }

    .project-description {
        font-size: 0.9rem;
    }

    .vote-count {
        font-size: 1rem;
    }

    .vote-button {
        padding: 8px 15px;
        font-size: 0.9rem;
    }

    .vote-message {
        font-size: 0.8rem;
    }

    .github-link {
        font-size: 1.3rem;
    }

    .app-link {
        font-size: 0.9rem;
        padding: 6px 12px;
    }

    .tech-tag {
        font-size: 0.7rem;
        padding: 3px 8px;
    }
}
</style>