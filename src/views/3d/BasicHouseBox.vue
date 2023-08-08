<template>
  <div>模板</div>
</template>

<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { FBXLoader } from "three/examples/jsm/loaders/FBXLoader";
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

// 加载模型并添加到场景中
const loadRoomModel = () => {
  const loader = new FBXLoader();
  loader.load("/assets/untitled.fbx", (fbx) => {
    console.log("fbx---", fbx);
    // 缩放倍数
    fbx.scale.set(0.01, 0.01, 0.01);
    scene.add(fbx);
  });
};

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
