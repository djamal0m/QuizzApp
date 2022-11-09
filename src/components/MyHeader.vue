<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled class="app_title">Quiz App</b-nav-item>
            <b-nav-item disabled>Counter: {{numCorrect}}/{{numTotal}} </b-nav-item>
        </b-nav>
    </div>
</template> 

<script>

export default {
    props: [
        'numCorrect',
        'numTotal',
    ]
}

</script>

<style scoped>
.app_title {
    font-size: 1.5rem;
    font-weight: bold;
}
</style>