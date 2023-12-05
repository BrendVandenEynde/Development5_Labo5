<template>
    <div>
        <ul >
            <li v-for="m in allMessages.data">
                <strong>{{ m.user }}</strong>
                <div>
                    {{ m.text  }}
                </div>
            </li>

        </ul>
    </div>

    <div>
        <input v-model="message" type="text" placeholder="Type your mesage here" @keyup.input.enter="sendMessage">
        <button @click="sendMessage">Send</button>
    </div>
</template>

<script setup>
    import { ref, reactive, onMounted} from 'vue';

    let message = ref("");

    let allMessages = reactive({
        data: {
                user:{
                    name: "user1",
                    text: "Hello"
                    },
                user2:{
                    name: "user2",
                    text: "Hi"
                    },
            },
    });

    function sendMessage() {
        if(message.value !== ""){
            allMessages.data.push(message.value);
        }
    }

    //get messages from https://dev5-lab4.onrender.com/api/v1/messages and add the user and the text to allMessages
    onMounted(() => {
        fetch('https://dev5-lab4.onrender.com/api/v1/messages')
        .then(response => response.json())
        .then(data => {
            console.log(data.data);
            allMessages.data = data.data[0].messages;
            console.log(allMessages.data);
        })
        .catch(error => {
            console.error(error);
        })

    });


</script>

<style scoped>

</style>