<template>
  <div>
    全景图贴图就是使用一张鱼眼全景图片以纹理的形式添加到球体上，如地球仪。
  </div>
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
camera.position.set(0, 0, 5);
scene.add(camera);
// 创建几何体
const url =
  "https://cdn.huodao.hk/upload_img/20220621/6bd594e62ea5654c03d7b82718443751.png?proportion=1.99";
const geometry = new THREE.SphereGeometry(5, 32, 32);
const texture = new THREE.TextureLoader().load(url);
const material = new THREE.MeshBasicMaterial({ map: texture });
const sphere = new THREE.Mesh(geometry, material);
sphere.geometry.scale(1, 1, -1);
scene.add(sphere);

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
