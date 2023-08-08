<template>
  <div>模板</div>
</template>

<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// 1、创建场景
const scene = new THREE.Scene();
// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
// 设置相机位置
camera.position.set(0, 0, 30);
scene.add(camera);
// 创建几何体
const geometry = new THREE.BoxGeometry(10, 10, 10);
const colors = [
  { color: 0xff0000 },
  { color: 0x00ff00 },
  { color: 0x0000ff },
  { color: 0xff00ff },
  { color: 0xffff00 },
  { color: 0x00ffff },
];
const material = [];
// 将6个面涂上不同的颜色
for (let i = 0, len = colors.length; i < len; i++) {
  material.push(new THREE.MeshBasicMaterial(colors[i]));
}
// 创建物体
var mesh = new THREE.Mesh(geometry, material);

// 将几何体添加到场景中
scene.add(mesh);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);
// // 使用渲染器，通过相机将场景渲染进来
renderer.render(scene, camera);

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);

// 启用惯性
controls.enableDamping = true;
// 相机向外移动极限
// controls.maxDistance = 4.5;

function render() {
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}
render();
</script>
<style scoped lang="less"></style>
