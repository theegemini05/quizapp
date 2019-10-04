<template>
    <div>
        <b-nav>
            <b-nav-item disabled>Quiz App</b-nav-item>
            <b-nav-item disabled>Counter {{counter}}/10</b-nav-item>
            <b-nav-item disabled>Marks: {{numCorrect}}/{{numTotal}}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props: {
        counter: Number,
        numCorrect: Number,
        numTotal: Number
    }
}
</script>