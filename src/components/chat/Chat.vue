<template>
  <div>
    <ul>
      <li v-for="message in allMessages" :key="message._id">
        <strong>{{ message.user }}</strong>
        <div>{{ message.text }}</div>
      </li>
    </ul>

    <div>
      <input v-model="newMessageText" type="text" placeholder="Type your message here" @keyup.enter="sendMessage">
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue';

let newMessageText = ref('');
let allMessages = reactive([]);

// Fetch messages from the API on component mount
onMounted(async () => {
  await fetchMessages();
});

// Function to fetch messages from the API
async function fetchMessages() {
  try {
    const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/');
    const data = await response.json();
    allMessages.value = data; // Use .value property for ref
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
      const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(newMessage),
      });

      const data = await response.json();
      // Update the messages array with the new message
      allMessages.value.unshift(data); // Use .value property for ref
      // Clear the input field
      newMessageText.value = ''; // Use .value property for ref
    } catch (error) {
      console.error('Error posting message:', error);
    }
  }
}
</script>

<style scoped>
/* Your styles go here */
</style>
