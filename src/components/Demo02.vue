<!--
* @Author: 陈国鹏
* @Date: 2025/09/02 09:20:30
* @Description: 画布-基础
* @Email chengp5@xiaopeng.com
!-->
<template>
  <div>
    <p>test</p>
    <img id="lyn-img" src="/lyn.jpg" width="100" height="100" alt="" />
    <canvas ref="canvas" id="canvas" width="800" height="500"></canvas>
  </div>
</template>

<script setup>
import { onMounted } from "vue";
import * as fabric from "fabric";
// import lynImage from "/public/lyn.jpg";

let fabricCanvas = null;

onMounted(() => {
  fabricCanvas = new fabric.Canvas("canvas");

  // 隐藏掉控制点
  // ['tl', 'bl', 'tr', 'br', 'ml', 'mb', 'mr', 'mt'].forEach(e => {
  //   fabric.Object.prototype.controls[e] = new fabric.Control({
  //     render: () => null,
  //   });
  // })
  // 全局设置：对所有新创建的对象隐藏旋转控制点和控制点
  fabric.FabricObject.prototype.setControlsVisibility({
    mtr: false,
    tl: false,
    bl: false,
    tr: false,
    br: false,
    ml: false,
    mb: false,
    mr: false,
    mt: false,
  });

  const circle = new fabric.Circle({
    top: 100, // y坐标
    left: 100, // x坐标
    fill: "#17b978", // 填充色
    radius: 50, // 半径
  });
  const text = new fabric.Textbox("Hello Fabric.js", {
    left: 100,
    top: 100,
    fontSize: 30,
    fill: "black",
    width: 10,
  });
  const rect = new fabric.Rect({
    left: 100,
    top: 200,
    fill: "red",
    width: 200,
    height: 100,
  });
  const line = new fabric.Line([50, 50, 200, 200], {
    left: 10,
    top: 10, //相当于基准点被移动到（10，10）
    stroke: "green",
    strokeWidth: 2, // 线条宽度
    selectable: false, // 不允许选中，默认是可以选中的
  });
  // 添加图片：方式1
  var imgElement = document.getElementById("lyn-img");
  var imgInstance = new fabric.FabricImage(imgElement, {
    left: 100,
    top: 100,
    angle: 30,
    width: 100,
    height: 100,
    originX: "center", //旋转时，旋转中心点
  });
  // 添加图片：方式2fromURL
  fabric.FabricImage.fromURL("/lyn4.png")
    .then((img) => {
      img.set({
        left: 100,
        top: 100,
        angle: 30,
        originX: "center",
        originY: "center",
      });

      // 按比例缩放（可选）
      img.scaleToWidth(100);

      fabricCanvas.add(img);
      fabricCanvas.renderAll();
    })
    .catch((error) => {
      console.error("图片加载失败:", error);
    });

  //路径，实例化path对象
  var path = new fabric.Path("M 0 0 L 200 100 L 170 200 z");//M 起始点 L线段 z结束
  path.set({ left: 300, top: 120 });
  path.set({ fill: 'red', stroke: 'green', opacity: 0.5 });

  fabricCanvas.add(circle, text, rect, line, imgInstance, path);
});
</script>

<style lang="less" scoped>
#canvas {
  border: 1px solid #ccc;
}
</style>
