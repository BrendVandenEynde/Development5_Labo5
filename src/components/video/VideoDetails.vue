<template>
    <div>
      <!-- Display the username and description -->
      <h3>{{ username }}</h3>
      <p>{{ description }}</p>
    </div>
  </template>
  
  <script setup>
  // Define the props (properties) expected by this component
  const props = defineProps(["description", "username"]);
  </script>
  
  <style scoped>
/*  Hier komt styling */
  </style>
  