<template>
  <div>
    <!-- Display messages using a loop -->
    <ul>
      <li v-for="message in allMessages" :key="message._id">
        <strong>{{ message.user }}</strong>
        <div>{{ message.text }}</div>
      </li>
    </ul>

    <!-- Input field and button for sending new messages -->
    <div>
      <input v-model="newMessageText" type="text" placeholder="Type your message here" @keyup.enter="sendMessage">
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

// Reactive variables for new messages and all messages
let newMessageText = ref('');
let allMessages = ref([]);

// Fetch messages from the API on component mount
onMounted(async () => {
  await fetchMessages();
});

// Function to fetch messages from the API
async function fetchMessages() {
  try {
    // Make an API request to fetch messages
    const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/');
    const data = await response.json();

    // Ensure data is an array before assigning to allMessages
    if (Array.isArray(data)) {
      allMessages.value = data;
    } else {
      console.error('Invalid API response. Expected an array of messages.');
    }
  } catch (error) {
    console.error('Error fetching messages:', error);
  }
}

// Function to post a new message to the API
async function sendMessage() {
  if (newMessageText.value.trim() !== '') {
    const newMessage = {
      user: 'Clueless', // Replace with actual username or get it dynamically
      text: newMessageText.value,
    };

    try {
      // Make an API request to post a new message
      const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(newMessage),
      });

      // Parse the response JSON
      const responseData = await response.json();

      if (response.ok) {
        // Update the messages array with the new message
        allMessages.value = [...allMessages.value, responseData.data];
        // Clear the input field
        newMessageText.value = '';
      } else {
        console.error('Error posting message:', responseData);
      }
    } catch (error) {
      console.error('Error posting message:', error);
    }
  }
}
</script>

<style scoped>
/*  Hier komt styling */
</style>
