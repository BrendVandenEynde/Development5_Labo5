<script setup>
// Import necessary components and functions from Vue
import VideoPlayer from '../src/components/video/VideoPlayer.vue';
import VideoDetails from '../src/components/video/VideoDetails.vue';
import Chat from './components/chat/Chat.vue';
import { ref, onMounted, reactive } from 'vue';

// Reactive variables
let newDescription = ref('Default description'); // Reference for the video description
let newUsername = ref(''); // Reference for the username
let messages = reactive([]); // Reactive array for chat messages

// Fetch messages from the API on component mount
onMounted(async () => {
  try {
    // Fetch messages from the specified API
    const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/');
    const data = await response.json();

    // Update the messages array with the fetched data
    messages.value = data;
  } catch (error) {
    console.error('Error fetching messages:', error);
  }
});

// Function to post a new message
const postNewMessage = async (newMessage) => {
  try {
    // Send a POST request to the API to add a new message
    const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(newMessage),
    });

    const data = await response.json();

    // Update the messages array with the new message at the beginning
    messages.unshift(data);
  } catch (error) {
    console.error('Error posting message:', error);
  }
};

// Handle the video update event
const updateVideo = (newVideoData) => {
  // Log the updated video data to the console
  console.log('Received updated video data:', newVideoData);
  
  // Update the description and username with the received data
  newDescription.value = newVideoData.description;
  newUsername.value = newVideoData.username;
};
</script>

<template>
  <div class="tiktok">
    <div>
      <!-- Display the VideoPlayer component and listen for video update events -->
      <VideoPlayer @update:video="updateVideo" />
    </div>
    <div>
      <!-- Display the VideoDetails component with dynamic bindings for description and username -->
      <VideoDetails :description="newDescription" :username="newUsername" />
      
      <!-- Display the Chat component with dynamic bindings for messages and postNewMessage function -->
      <Chat :messages="messages" :postNewMessage="postNewMessage" />
    </div>
  </div>
</template>

<style scoped>
  /* Apply scoped styles to the components */
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
