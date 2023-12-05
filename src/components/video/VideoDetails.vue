<template>
    <div>
        <h3>{{ username }}</h3>
        <p>{{ description }}</p>
    </div>
</template>

<script setup>
    const props = defineProps(["description", "username"]);
</script>

<style scoped>
    
</style>
