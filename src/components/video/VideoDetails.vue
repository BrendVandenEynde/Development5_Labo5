<template>
    <div>
        <h3>Username</h3>
        <p>{{ description }}</p>
    </div>
</template>

<script setup>
    const props = defineProps(["description"]);
</script>

<style scoped>
    
</style>