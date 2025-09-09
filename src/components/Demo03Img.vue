<!--
* @Author: 陈国鹏
* @Date: 2025/09/02 09:20:30
* @Description: 画布-动画，颜色，文本，事件
* @Email chengp5@xiaopeng.com
!-->
<template>
  <div>
    <p>test</p>
    <canvas ref="canvas" id="canvas" width="800" height="500"></canvas>
  </div>
</template>

<script setup>
import { onMounted } from "vue";
import * as fabric from "fabric";

let fabricCanvas = null;

onMounted(() => {
  fabricCanvas = new fabric.Canvas("canvas");

  const text = new fabric.FabricText("hello world", { left: 200, top: 100 });

  const rect = new fabric.Rect({
    left: 100,
    top: 100,
    width: 100,
    height: 100,
    fill: "('000').toHex();",
    // color: "('fff').toHex(); ",
  });

  fabricCanvas.add(rect, text);

  //动画1
  text.animate(
    {
      left: 300,
      top: 200,
      fontSize: 48,
      fill: "blue",
    },
    {
      duration: 2000, // 设置动画的持续时间为 2000 毫秒
      onChange: fabricCanvas.renderAll.bind(fabricCanvas), // 在动画过程中更新 canvas
      easing: fabric.util.ease.easeInQuad,
      onComplete: () => {
        console.log("11111");
      },
    }
  );

  //动画2
  rect.animate(
    {
      left: 100,
      top: 300,
      fill: "blue",
    },
    {
      duration: 2000, // 设置动画的持续时间为 2000 毫秒
      onChange: fabricCanvas.renderAll.bind(fabricCanvas), // 在动画过程中更新 canvas
      easing: fabric.util.ease.easeInQuad,
      onComplete: () => {
        console.log("22222");
      },
    }
  );

  //灰度滤镜
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

      // 添加滤镜
      img.filters.push(new fabric.filters.Grayscale());

      // 应用滤镜
      img.applyFilters();

      fabricCanvas.add(img);
      fabricCanvas.renderAll();
    })
    .catch((error) => {
      console.error("图片加载失败:", error);
    });

  var circle = new fabric.Circle({
    left: 100,
    top: 100,
    radius: 50,
  });
  var gradient = new fabric.Gradient({
    type: "linear",
    gradientUnits: "pixels", // or 'percentage'
    coords: { x1: 0, y1: 0, x2: 0, y2: circle.height },
    // colorStops:
  });
  circle.set("fill", gradient);
  fabricCanvas.add(circle);
});
</script>

<style lang="less" scoped>
#canvas {
  border: 1px solid #ccc;
}
</style>
