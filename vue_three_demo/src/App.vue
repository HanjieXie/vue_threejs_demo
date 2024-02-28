<template>
  <div id="container"></div>
</template>

<script setup>
import * as THREE from 'three';
import { onMounted, ref } from 'vue';
import * as dat from 'dat.gui';
// 初始化Threejs
const initThreeJS = () => {
  // 获取屏幕宽度和高度
  const width = window.innerWidth;
  const height = window.innerHeight;
  const AspectRatio = width / height; //窗口宽高比
  const Range = 200; //三维场景显示范围控制系数，系数越大，显示的范围越大
  // ===== 1.创建 Threejs 场景 =====
  const scene = new THREE.Scene;
  // ===== 2.创建 Camera 相机 =====
  const camera = new THREE.OrthographicCamera(-Range * AspectRatio, Range * AspectRatio, Range, -Range, 1, 1000);
  // ===== 3.相机位置 =====
  camera.position.set(200, 300, 200);
  // ===== 4.相机方向 =====
  camera.lookAt(scene.position);
  // ===== 5.创建渲染对象 =====
  const renderer = new THREE.WebGLRenderer();
  // ===== 6.设置渲染区域尺寸，背景颜色 =====
  renderer.setSize(width, height);
  renderer.setClearColor(0xb9d3ff, 1);
  document.getElementById('container').appendChild(renderer.domElement);

  // ===== 7.点光源位置 =====
  const point = new THREE.PointLight(0xffffff);
  point.position.set(400, 200, 300);
  // ===== 8.点光源添加到场景中 =====
  scene.add(point);
  const ambient = new THREE.AmbientLight(0x444444);
  scene.add(ambient);
  // ===== 8.几何体 =====
  const geometry = new THREE.BoxGeometry(100, 100, 100);
  const material = new THREE.MeshBasicMaterial({
    color: 0x0000ff, //设置材质颜色
    transparent: true, //开启透明
    opacity: 0.5, //设置透明度
  });
  const mesh = new THREE.Mesh(geometry, material);
  scene.add(mesh);
  // 辅助观察坐标系
  const axesHelper = new THREE.AxesHelper(150);
  scene.add(axesHelper);

  // 渲染场景
  function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
  }

  animate();
}


onMounted(() => {
  initThreeJS();
})
</script>
<style scoped>
#container {
  width: 100vw;
  height: 100vh;
}
</style>
