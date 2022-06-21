<template>
  <div id="app">
    <VideoStream
        video-id="5d5bc37ffcf54c9b82e996823bffbb81"
        class="video"
        autoplay muted loop
    />
  </div>
</template>

<script>
import Vue from 'vue';
import VideoStream from '@/VideoStream.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VideoStream
  }
});
</script>

<style>
.video {
  object-fit: cover;
  width: 100%;
  height: 100%;
}
</style>
