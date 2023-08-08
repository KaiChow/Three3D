<template>
  <div>天空盒贴图</div>
</template>

<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
/**
 * 根据我的了解目前常用于实现全景看房效果的有两种，分别纹理贴图和3D建模。

纹理贴图类全景看房又分为 天空盒贴图 和 全景图片贴图

天空盒贴图
天空盒的原理是将我们所处的场景看成是有前后、左右、上下6个面组成的，将我们所看到的这6个面的视觉镜像处理成图片，将其分别以纹理的形式添加到立方体中，这时，我们如果立身于这个立方体中，即可还原当时场景。
 */
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
const geometry = new THREE.BoxGeometry(10, 10, 10);
// 左右、上下、后前
const urls = [
  "https://cdn.huodao.hk/upload_img/20220620/3e532822bd445485d27677ca55a79b10.jpg?proportion=1",
  "https://cdn.huodao.hk/upload_img/20220620/cebf6fbcafdf4f5c945e0881418e34ec.jpg?proportion=1",
  "https://cdn.huodao.hk/upload_img/20220620/273081d1896fc66866842543090916d3.jpg?proportion=1",
  "https://cdn.huodao.hk/upload_img/20220620/8747f61fd2215aa748dd2afb6dce3822.jpg?proportion=1",
  "https://cdn.huodao.hk/upload_img/20220620/c34262935511d61b2e9f456b689f5c1c.jpg?proportion=1",
  "https://cdn.huodao.hk/upload_img/20220620/722d2bf88f6087800ddf116511b51e73.jpg?proportion=1",
];
const boxMaterial = [];
urls.forEach((item) => {
  // 纹理加载---使用.TextureLoader()加载图片，转化为纹理，通过属性map设置材质纹理。就实现了简单的纹理加载。
  const texture = new THREE.TextureLoader().load(item);
  // 通过旋转修复天花板和地板
  if (item == "4_u" || item == "4_d") {
    texture.rotation = Math.PI;
    texture.center = new THREE.Vector2(0.5, 0.5);
  }
  // 创建材质
  boxMaterial.push(new THREE.MeshBasicMaterial({ map: texture }));
});
const house = new THREE.Mesh(geometry, boxMaterial);
house.geometry.scale(1, 1, -1);
scene.add(house);

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
controls.maxDistance = 4.5;

function render() {
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}
render();
</script>
<style scoped lang="less"></style>
