<template>
    <div>
        <video :src="videoUrl" controls autoplay muted></video>
    </div>
</template>

<script setup>
import { ref, reactive, onMounted} from 'vue';

const emit = defineEmits(["update:video"]);    

let videoUrl = ref("");

let videos = reactive({
    data: [],
});
onMounted(() => {
    fetch("https://api.jsonbin.io/v3/b/6548ef9954105e766fcc2c15")
    .then((res) => res.json())
    .then((data) => {
        videos.data = data.record.videos;
        const videoData = videos.data[0];
        videoUrl.value = videoData.video;
        emit("update:video", videoData);
    });
});
</script>


<style scoped>
    video{
        width: 100%;
        max-height: 70vh;
    }
</style>