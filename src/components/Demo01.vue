<template>
  <div>
    <canvas id="c"></canvas>
  </div>
</template>

<script setup>
import { onMounted } from "vue";
import * as fabric from "fabric";

onMounted(() => {
  // var canvas = new fabric.Canvas("c");
  // var rect = new fabric.Rect();

  // rect.set({
  //   left: 100,
  //   top: 100,
  //   fill: "red",
  //   width: 100,
  //   height: 100,
  // });

  var staticCanvas = new fabric.StaticCanvas("c");

  staticCanvas.add(
    new fabric.Rect({
      width: 10,
      height: 20,
      left: 100,
      top: 100,
      fill: "yellow",
      angle: 30,
    })
  );

  canvas.selection = false; // 设置不能群组选择
  rect.set("selectable", false); // 设置rect对象无法被选中

  // canvas.add(rect);
  canvas.renderAll();
});
</script>

<style scoped></style>
