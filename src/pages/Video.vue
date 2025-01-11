<template>
  <!-- Container for our video.js player -->
  <div data-vjs-player>
    <video ref="videoPlayer" class="video-js" playsinline>
    </video>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount, watch } from 'vue'
import videojs from 'video.js'
// Import the default styling for video.js
import 'video.js/dist/video-js.css'

export default {
  name: 'VideoPlayer',
  props: {
    src: {
      type: String,
      required: true,
    },
    controls: {
      type: Boolean,
      default: false,
    },
    autoplay: {
      type: Boolean,
      default: true,
    },
    loop: {
      type: Boolean,
      default: true,
    },
    muted: {
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    const videoPlayer = ref(null)
    const playerInstance = ref(null)

    // Options similar to the ones in your original code
    const options = {
      width: 640, // Fixed width
      height: 360, // Fixed height
      fill: false,
      fluid: false,
      preload: 'meta',
      html5: {
        hls: {
          enableLowInitialPlaylist: true,
          smoothQualityChange: true,
          overrideNative: true,
        },
      },
    }

    onMounted(() => {
      // Initialize the video.js player
      playerInstance.value = videojs(videoPlayer.value, {
        ...options,
        controls: true,
        autoplay: true,
        loop: true,
        muted: true,
        sources: [props.src],
      })
    })

    // Dispose the player on unmount
    onBeforeUnmount(() => {
      if (playerInstance.value) {
        playerInstance.value.dispose()
      }
    })

    // Watch for changes on `src` and update the player source
    watch(
      () => props.src,
      (newSrc) => {
        if (playerInstance.value) {
          playerInstance.value.src({ src: newSrc })
        }
      }
    )

    return {
      videoPlayer,
    }
  },
}
</script>

<style scoped>
/* You can further customize the .video-js or container styling here */
</style>
