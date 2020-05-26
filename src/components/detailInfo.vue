<template>
    <ul v-if="show">
        <li>Name: {{detal.name}}</li>
        <li>Id: {{detal.id}}</li>
        <li>Email: {{detal.email}}</li>
        <li>Location: {{detal.location}}</li>
    </ul>
</template>

<script>
export default {
    props: ['detal', 'show']
}
</script>

<style scoped>
    ul {
        position: absolute;
        background-color: #fff;
        padding: 5px;
        top: 140px;
        z-index: 1;
    }
</style>