<script setup>
  import VideoPlayer from './components/video/VideoPlayer.vue';
  import VideoDetails from './components/video/VideoDetails.vue';
  import Chat from './components/chat/Chat.vue';

  import { ref } from 'vue';
  let newDescription = ref("Default description");

  const updateVideo = (videoData) => {
    newDescription.value = videoData.description;
    console.log(videoData);
    console.log(videoData.description);
  }
  
</script>

<template>
    <div class="tiktok">
        <div>
            <VideoPlayer @update:video="updateVideo" />
        </div>
        <div>
            <VideoDetails :description="newDescription"/>
            <chat/>
        </div>
    </div>
</template>

<style scoped>
    .tiktok {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        width: 100%;
        height: 100%;
    }
    div{
      margin: 50px;
    }
</style>
