**Vue之photo-sphere-viewer全景图**
==========

### 安装 photo-sphere-viewer依赖
```
npm install  photo-sphere-viewer --save-dev 
```

### 在你需要用到的页面引入文件
```
import PhotoSphereViewer from 'photo-sphere-viewer'
import 'photo-sphere-viewer/dist/photo-sphere-viewer.css'
```
### 代码示例
```
<template>
  <div>
    <div class="viewer" id="viewer"></div>
  </div>
</template>
<script>
import PhotoSphereViewer from "photo-sphere-viewer";
import "photo-sphere-viewer/dist/photo-sphere-viewer.css";
export default {
  data() {
    return {
      factoryLink: "http://img8.zol.com.cn/bbs/upload/20274/20273871.JPG",
      PSV: "",
      img: ""
    };
  },
  mounted() {
    this.initPhotoSphere();
  },
  methods: {
    initPhotoSphere() {
      this.PSV = PhotoSphereViewer({
        container: document.getElementById("viewer"),
        panorama: this.factoryLink,
        size: {
          width: "200px",
          height: "200px"
        },
        // caption: '厂区鸟瞰图',
        caption: "  ",
        with_credentials: true,
        time_anim: false,
        default_long: 1.4441088145446443,
        default_lat: 0.0800613513013615,
        sphere_correction: { pan: 30.01, tilt: 0, roll: 0 },
        // max_fov: 100,         // 最大缩放值
        // min_fov: 99,          // 最小缩放值
        default_fov: 100, // 默认缩放值，在1-179之间
        // latitude_range: [0,0],//禁止上下滑动
        // mousewheel: false,    // 禁止鼠标滚轮缩放
        // navbar: false,
        navbar: ["autorotate", "zoom", "markers", "caption", "fullscreen"],
        theta_offset: 1000 // 旋转速度
        // markers: this.markersData
      });
    }
  }
};
</script>
<style lang="less">
.viewer {
  width: 200px;
  height: 200px;
}
</style>
```
### 一些方法
> destroy():销毁全景图对象
```
this.PSV.destroy()
```
> setPanorama():为全景图对象添加图片路径
```
this.PSV.setPanorama(this.factoryLink, true, true)
```