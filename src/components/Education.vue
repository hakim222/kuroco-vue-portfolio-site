<template>
    <section id="education">
        <h2>Educations</h2>
        <div v-if="loading">Loading...</div>
        <div v-else-if="error">{{ error }}</div>
        <draggable v-else v-model="educations" @end="updateOrder" item-key="id">
            <template #item="{ element }">
                <EducationCard :education="element" />
            </template>
        </draggable>
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
    padding: 4rem 100px 0px;
}

h2 {
    text-align: center;
    margin-bottom: 3rem;
    font-size: 2.5rem;
    font-weight: 300;
    letter-spacing: 2px;
}

#education {
    display: flex;
    flex-direction: column;
    align-items: center;
}
</style>
