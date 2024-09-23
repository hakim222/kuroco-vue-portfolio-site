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
        upvote() {
            this.project.votes++
           
        },
        downvote() {
            this.project.votes--
        }
    }
}
</script>

<style>

</style>