<template>
  <video ref="streamEl" v-bind="computedProps" />
</template>

<script>
import Hls from 'hls.js'

export default {
  name: 'VideoStream',
  props: {
    /**
     * The video id for the video you’ve uploaded to Cloudflare Stream should be included here.
     */
    videoId: {
      type: String,
      default: null
    },
    /**
     * Tells the browser to immediately start downloading the video and play it as soon as it can. Note that mobile browsers generally do not support this attribute, the user must tap the screen to begin video playback. Please consider mobile users or users with Internet usage limits as some users don’t have unlimited Internet access before using this attribute.
     *
     * To disable video autoplay, the autoplay attribute needs to be removed altogether as this attribute. Setting autoplay="false" will not work; the video will autoplay if the attribute is there in the <stream> tag.
     *
     * In addition, some browsers now prevent videos with audio from playing automatically. You may add the mute attribute to allow your videos to autoplay. For more information, [go here](https://webkit.org/blog/6784/new-video-policies-for-ios/).
     *
     * @default false
     */
    autoplay: {type: Boolean, default: false},
    /**
     * Shows the default video controls such as buttons for play/pause, volume controls. You may choose to build buttons and controls that work with the player.
     *
     * @default false
     */
    controls: {type: Boolean, default: false},
    /**
     * Returns the current playback time in seconds. Setting this value seeks the video to a new time.
     *
     * @default 0
     */
    currentTime: {type: Number, default: 0},
    /**
     * A Boolean attribute which indicates the default setting of the audio contained in the video. If set, the audio will be initially silenced.
     *
     * @default false
     */
    muted: {type: Boolean, default: false},
    /**
     * A Boolean attribute; if included the player will automatically seek back to the start upon reaching the end of the video.
     *
     * @default false
     */
    loop: {type: Boolean, default: false},
    /**
     * This enumerated attribute is intended to provide a hint to the browser about what the author thinks will lead to the best user experience. You may choose to include this attribute as a boolean attribute without a value, or you may specify the value preload="auto" to preload the beginning of the video. Not including the attribute or using preload="metadata" will just load the metadata needed to start video playback when requested.
     *
     * The <video> element does not force the browser to follow the value of this attribute; it is a mere hint. Even though the preload="none" option is a valid HTML5 attribute, Stream player will always load some metadata to initialize the player. The amount of data loaded in this case is negligable.
     */
    preload: {
      type: [String, Boolean],
      default: 'auto',
      validator(value) {
        return ['auto', 'metadata', 'none'].includes(value) || (typeof value === 'boolean' && !!value)
      }
    },
    /**
     * Sets volume from 0.0 (silent) to 1.0 (maximum value)
     *
     * @default 1
     */
    volume: {type: Number, default: 1},
  },
  data() {
    return {
      hls: null
    }
  },
  mounted() {
    this.initPlayer()
  },
    computed: {
        computedProps () {
            return {
                autoplay: this.autoplay,
                controls: this.controls,
                muted: this.muted,
                loop: this.loop,
                preload: this.preload,
                volume: this.volume
            }
        }
    },
  methods: {
    initPlayer() {
      if (!this.$refs.streamEl) return

      this.hls = new Hls()
      this.hls.loadSource(`https://videodelivery.net/${this.videoId}/manifest/video.m3u8`)
      this.hls.attachMedia(this.$refs.streamEl)
    }
  }
}
</script>

<style>
.vjs-error-display .vjs-modal-dialog-content {
  display: none
}
</style>
