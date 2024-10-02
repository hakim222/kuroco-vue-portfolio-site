<template>
    <section id="education">
        <h2>Educations</h2>
        <div v-if="loading">Loading...</div>
        <div v-else-if="error" class="error-message">{{ error }}</div>
        <div v-else>
            <div class="drag-indicator">
                <i class="fas fa-arrows-alt-v"></i>
                Drag items to change order!
            </div>
            <div class="education-list-container">
                <div v-if="updating" class="overlay">
                    <p class="vote-message voting">Updating order...</p>
                </div>
                <draggable v-model="educations" @end="updateOrder" item-key="id" class="education-list">
                    <template #item="{ element }">
                        <EducationCard :education="element" />
                    </template>
                </draggable>
            </div>
        </div>
    </section>
</template>

<script>
import draggable from 'vuedraggable';
import EducationCard from './EducationCard.vue';

export default {
    components: {
        draggable,
        EducationCard
    },
    data() {
        return {
            educations: [],
            loading: true,
            error: null,
            updating: false,
            updateMessage: null,
            updateError: false
        };
    },
    mounted() {
        this.fetchEducations();
    },
    methods: {
        fetchEducations() {
            fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/educations')
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then(data => {
                    this.educations = data.list.sort((a, b) => a.order - b.order);
                    this.loading = false;
                })
                .catch(err => {
                    this.error = 'Failed to fetch education data';
                    this.loading = false;
                    console.error('Error fetching educations:', err);
                });
        },
        updateOrder() {

            const hasOrderChanged = this.educations.some((edu, index) => edu.order !== index + 1);

            if (!hasOrderChanged) {
                console.log('Order unchanged, skipping update');
                return;
            }

            this.updating = true;
            this.updateMessage = null;
            this.updateError = false;

            const updatedEducations = this.educations.map((edu, index) => ({
                topics_id: edu.topics_id,
                order: index + 1
            }));

            fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/reordereducations', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ list: updatedEducations })
            })
            .then(response => {
                if (!response.ok) {
                    throw new Error('Failed to update education order');
                }
                return this.fetchEducations();
            })
            .then(() => {
                this.updateMessage = 'Update completed successfully!';
                setTimeout(() => {
                    this.updateMessage = null;
                }, 3000);
            })
            .catch(err => {
                console.error('Failed to update education order:', err);
                this.updateMessage = 'Failed to update education order';
                this.updateError = true;
            })
            .finally(() => {
                this.updating = false;
            });
        }
    }
}
</script>

<style scoped>
section {
    color: #e0e0e0;
}

.education-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 2rem; 
    padding: 1rem;
}

.education-list::-webkit-scrollbar,
.education-list::-webkit-scrollbar-track,
.education-list::-webkit-scrollbar-thumb {
    display: none;
}

.drag-indicator {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    padding: 0.75rem;
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 4px;
    font-size: 1rem;
    margin-bottom: 1.5rem;
    font-weight: bold;
    text-transform: uppercase;
    letter-spacing: 1px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
}

.drag-indicator:hover {
    background-color: rgba(255, 255, 255, 0.2);
    transform: translateY(-2px);
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.drag-indicator i {
    font-size: 1.2rem;
}

.education-list-container {
    position: relative;
}

.overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.7);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
}

.vote-message {
    font-size: 1rem;
    font-weight: bold;
    color: #ffffff;
    text-align: center;
    padding: 8px 15px;
    border-radius: 20px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    margin: 10px 0;
}

.vote-message::before {
    font-family: 'Font Awesome 5 Free';
    margin-right: 5px;
}

.vote-message.voting {
    background-color: rgba(60, 60, 60, 0.6);
    border: 1px solid rgba(255, 255, 255, 0.4);
}

.vote-message.voting::before {
    content: '\f110';
    animation: fa-spin 2s infinite linear;
}

.vote-message.error {
    background-color: rgba(78, 0, 0, 0.6);
    border: 1px solid rgba(255, 99, 71, 0.4);
    color: rgb(180, 74, 55);
}

@keyframes fa-spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

.error-message {
    color: #ff6b6b;
    text-align: center;
    padding: 1rem;
    background: rgba(255, 107, 107, 0.1);
    border-radius: 10px;
    margin-top: 1rem;
}

@media (max-width: 480px) {
    .education-list {
        flex-direction: column;
        align-items: center;
        gap: 1rem;
        padding: 0.5rem;
    }

    .drag-indicator {
        font-size: 0.9rem;
        padding: 0.5rem;
        margin-bottom: 1rem;
    }

    .drag-indicator i {
        font-size: 1rem;
    }

    .vote-message {
        font-size: 0.9rem;
        padding: 6px 12px;
    }
}
</style>