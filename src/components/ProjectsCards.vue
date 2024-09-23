<template>
    <div>
        <h2>{{project.subject}}</h2>
        <p>{{project.description}}</p>
        <p>Votes: {{ project.votes }}</p>
        <button @click="upvote">Vote</button>
        <button @click="downvote">Downvote</button>
    </div>
</template>

<script>
export default {
    props: ['project'],
    methods: {
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
            } catch (error) {
                console.error(error)
            }
        }
    }
}
</script>

<style>

</style>