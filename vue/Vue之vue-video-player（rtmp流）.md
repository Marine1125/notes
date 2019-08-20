# **Vue 之 vue-video-player 视频播放（rtmp 流）**

### 安装 vue-video-player

```
npm install vue-video-player
```

> 注意：由于 vue-video-player 插件会依赖 videojs-flash 和 video.js 插件，在安装的时候他会自行为你安装这两个插件，如果你之前安装过这两个插件，请先删除，然后在安装 vue-video-player，否则可能在使用 vue-video-player 的时候会报莫名其妙的错误。

### 示例代码

```
<template>
	<videoPlayer class="vjs-custom-skin videoPlayer" :options="playerOptions"></videoPlayer>
</template>
<script>
    import 'video.js/dist/video-js.css'
    import {videoPlayer} from 'vue-video-player'
    import 'videojs-flash'
    export default {
    components: {
        videoPlayer,
    },
    data() {
        return {
        playerOptions: {
            height: '300',
            sources: [{
            type: "rtmp/mp4",
            src: "rtmp://live.hkstv.hk.lxdns.com/live/hks"
            }],
            techOrder: ['flash'],
            autoplay: false,
            controls: true
        }
        }
    }
    }
</script>
```
