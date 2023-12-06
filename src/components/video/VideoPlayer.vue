<template>
    <div>
      <!-- Display a video element with the specified video source -->
      <video :src="videoUrl" controls autoplay muted></video>
    </div>
  </template>
  
  <script setup>
  // Import necessary functions and reactive variables from Vue
  import { ref, reactive, onMounted } from 'vue';
  
  // Define a custom event for emitting video updates to the parent component
  const emit = defineEmits(["update:video"]);
  
  // Create a ref to store the video URL
  let videoUrl = ref("");
  
  // Create a reactive object to store video data
  let videos = reactive({
      data: [],
  });
  
  // On component mount, fetch video data from a JSON bin and update the video URL
  onMounted(() => {
      // Fetch video data from a JSON bin
      fetch("https://api.jsonbin.io/v3/b/6548ef9954105e766fcc2c15")
      .then((res) => res.json())
      .then((data) => {
          // Store the fetched video data
          videos.data = data.record.videos;
          
          // Get the first video data
          const videoData = videos.data[0];
          
          // Set the video URL
          videoUrl.value = videoData.video;
  
          // Emit a custom event to update the parent component with the video data
          emit("update:video", videoData);
      });
  });
  </script>
  
  <style scoped>
  /* Apply styles to the video element */
  video {
      width: 100%;
      max-height: 70vh;
  }
  </style>
  