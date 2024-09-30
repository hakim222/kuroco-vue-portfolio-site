<template>
    <section id="education">
        <h2>Educations</h2>
        <div v-if="loading">Loading...</div>
        <div v-else-if="error">{{ error }}</div>
        <div v-else>
            <div class="drag-indicator">
                <i class="fas fa-arrows-alt-v"></i>
                Drag items to change order!
            </div>
            <draggable v-model="educations" @end="updateOrder" item-key="id" class="education-list">
                <template #item="{ element }">
                    <EducationCard :education="element" />
                </template>
            </draggable>
        </div>
    </section>
</template>

<script>
import { ref, onMounted } from 'vue';
import draggable from 'vuedraggable';
import EducationCard from './EducationCard.vue';

export default {
    components: {
        draggable,
        EducationCard
    },
    setup() {
        const educations = ref([]);
        const loading = ref(true);
        const error = ref(null);

        const fetchEducations = async () => {
            try {
                const response = await fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/educations');
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                const data = await response.json();
                educations.value = data.list.sort((a, b) => a.order - b.order);
                loading.value = false;
            } catch (err) {
                error.value = 'Failed to fetch education data';
                loading.value = false;
            }
        };

        const updateOrder = async () => {
            const updatedEducations = educations.value.map((edu, index) => ({
                topics_id: edu.topics_id,
                order: index + 1 // Update the order based on the new index
            }));

            try {
                const response = await fetch('https://hakim-azizan.g.kuroco.app/rcms-api/1/reordereducations', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({ list: updatedEducations })
                });

                if (!response.ok) {
                    throw new Error('Failed to update education order');
                }

                // Optionally, refresh the educations list after successful update
                await fetchEducations();
            } catch (err) {
                console.error('Failed to update education order:', err);
            }
        };

        onMounted(fetchEducations);

        return {
            educations,
            loading,
            error,
            updateOrder
        };
    }
}
</script>

<style scoped> 
section {
    color: #e0e0e0;
    padding: 4rem 0px 2rem;
}

h2 {
    text-align: center;
    margin: 3rem 0rem;
    font-size: 2.5rem;
    font-weight: 300;
    letter-spacing: 2px;
}

#education {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.education-list {
    width: 100%;
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
</style>
