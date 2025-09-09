<!--
* @Author: 陈国鹏
* @Date: 2025/09/02 09:20:30
* @Description: 画布-动画，颜色，文本，事件，动画，图片，滤镜，渐变
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

  //文本的一些属性
  const text = new fabric.FabricText("hello world", {
    left: 200,
    top: 100,
    fontFamily: "Comic Sans",
    fontSize: 40,
    fontWeight: "bold",
    linethrough: true,
    shadow: "green -5px -5px 3px",
    fontStyle: "italic",
    fontFamily: "Hoefler Text",
  });

  const rect = new fabric.Rect({
    left: 100,
    top: 100,
    width: 100,
    height: 100,
    fill: "pink",
  });
  //事件可以直接添加在画布的对象上
  rect.on("selected", function () {
    console.log("selected a rectangle");
  });

  fabricCanvas.add(rect, text);

  //动画1
  text.animate(
    {
      left: 300,
      top: 200,
    },
    {
      duration: 2000, // 设置动画的持续时间为 2000 毫秒
      onChange: fabricCanvas.renderAll.bind(fabricCanvas), // 在动画过程中更新 canvas
      easing: fabric.util.ease.easeInQuad, // 设置缓动效果
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

  //滤镜
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
      // img.filters.push(new fabric.filters.Grayscale());
      img.filters.push(
        // new fabric.filters.Grayscale(),//灰度滤镜
        // new fabric.filters.Invert(),//反相滤镜
        new fabric.filters.Blur({ blur: 0.5 }) //模糊滤镜
        // new fabric.filters.Sepia(),//棕褐色复古滤镜
        // new fabric.filters.Brightness({ brightness: 10 })//增加亮度
      );

      // 应用所有滤镜
      img.applyFilters();

      // 应用滤镜
      img.applyFilters();

      fabricCanvas.add(img);
      fabricCanvas.renderAll();
    })
    .catch((error) => {
      console.error("图片加载失败:", error);
    });

  // 渐变
  var circle = new fabric.Circle({
    left: 200,
    top: 10,
    radius: 50,
  });
  //线性渐变
  var gradient1 = new fabric.Gradient({
    type: "linear", // 线性渐变
    gradientUnits: "pixels", // pixels or pencentage 像素 或者 百分比
    coords: { x1: 0, y1: 0, x2: circle.width, y2: 0 }, // 至少2个坐标对(x1，y1和x2，y2)将定义渐变在对象上的扩展方式
    colorStops: [
      // 定义渐变颜色的数组
      { offset: 0, color: "red" },
      { offset: 0.2, color: "pink" },
      { offset: 0.4, color: "yellow" },
      { offset: 0.6, color: "green" },
      { offset: 0.8, color: "blue" },
      { offset: 1, color: "purple" },
    ],
  });
  //径向渐变
  let gradient2 = new fabric.Gradient({
    type: "radial",
    coords: {
      r1: 50, // 该属性仅径向渐变可用，外圆半径
      r2: 0, // 该属性仅径向渐变可用，外圆半径
      x1: 50, // 焦点的x坐标
      y1: 50, // 焦点的y坐标
      x2: 50, // 中心点的x坐标
      y2: 50, // 中心点的y坐标
    },
    colorStops: [
      { offset: 0, color: "#fee140" },
      { offset: 1, color: "#fa709a" },
    ],
  });

  // 设置填充为渐变
  circle.set("fill", gradient2);

  fabricCanvas.add(circle);

  //事件
  //鼠标级别
  fabricCanvas.on("mouse:down", function (options) {
    if (options.target) {
      console.log("an object was clicked! ", options.target.type); //an object was clicked!  rect
    }
  });
});
</script>

<style lang="less" scoped>
#canvas {
  border: 1px solid #ccc;
}
</style>
