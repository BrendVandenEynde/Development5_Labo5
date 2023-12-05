<script setup>
  import VideoPlayer from '../src/components/video/VideoPlayer.vue';
  import VideoDetails from './components/video/VideoDetails.vue';
  import Chat from './components/chat/Chat.vue';
  import { ref, onMounted, reactive } from 'vue';

  // Reactive variables
  let newDescription = ref("Default description");
  let messages = reactive([]);

 // Fetch messages on component mount
onMounted(async () => {
  try {
    const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/');
    const data = await response.json();
    messages.value = data;
  } catch (error) {
    console.error('Error fetching messages:', error);
  }
});

// Function to post a new message
const postNewMessage = async (newMessage) => {
  try {
    const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(newMessage),
    });

    const data = await response.json();
    // Update the messages array with the new message
    messages.unshift(data);
  } catch (error) {
    console.error('Error posting message:', error);
  }
};


</script>

<template>
  <div class="tiktok">
    <div>
      <VideoPlayer @update:video="updateVideo" />
    </div>
    <div>
      <VideoDetails :description="newDescription" />
      <Chat :messages="messages" :postNewMessage="postNewMessage" />
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

  div {
    margin: 50px;
  }
</style>
