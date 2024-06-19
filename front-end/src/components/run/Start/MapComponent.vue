<script setup>
import { onMounted, onUnmounted, ref } from "vue";
import AMapLoader from "@amap/amap-jsapi-loader";

let map = null;
let marker = null;

// 定义props，接收经纬度参数
const props = defineProps({
  center: {
    type: Array,
    required: true
  }
});

onMounted(() => {
  window._AMapSecurityConfig = {
    securityJsCode: "4827157a8f21474940bda160cff56dc6",
  };
  AMapLoader.load({
    key: "30c315e38d696ee544ff4f9e2e53c4c7", // 申请好的Web端开发者Key，首次调用 load 时必填
    version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
    plugins: ["AMap.Scale"], //需要使用的的插件列表，如比例尺'AMap.Scale'，支持添加多个如：['...','...']
  })
      .then((AMap) => {
        map = new AMap.Map("container", {
          // 设置地图容器id
          viewMode: "3D", // 是否为3D地图模式
          zoom: 11, // 初始化地图级别
          center: props.center, // 使用props中的center作为地图中心点
          mapStyle: "amap://styles/fresh", //设置地图的显示样式
        });

        // 创建标记并添加到地图上
        marker = new AMap.Marker({
          position: props.center, // 标记的位置
          title: '指定位置' // 标记的标题
        });
        map.add(marker);
      })
      .catch((e) => {
        console.log(e);
      });
});

onUnmounted(() => {
  map?.destroy();
  marker?.destroy();
});
</script>

<template>
  <div class="content">
    <div id="container"></div>
  </div>

</template>

<style scoped>
#container {
  width: 100%;
  height: 92vh;
}
</style>