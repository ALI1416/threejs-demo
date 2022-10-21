<template>
</template>

<style>
body {
  margin: 0;
}
</style>

<script>
/* 0、安装并导入依赖 */
import * as THREE from 'three';
import {OrbitControls} from "three/examples/jsm/controls/OrbitControls.js"

/* 1、创建场景 */
const scene = new THREE.Scene();

/* 2、创建相机 */
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.set(0, 0, 10);
scene.add(camera);

/* 3、创建物体 */
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
// 纹理下载
// https://www.poliigon.com/
// https://3dtextures.me/
// https://www.arroway-textures.ch/
// https://quixel.com/bridge 安装Bridge使用虚幻引擎账号登录后免费下载
// 导入纹理
const textureLoader = new THREE.TextureLoader();
const doorColorTextureLoader = textureLoader.load("./textures/door/color.jpg");
const cubeMaterial = new THREE.MeshBasicMaterial({
  color: 0x66ccff,
  map: doorColorTextureLoader,
});
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
scene.add(cube);

/* 4、创建渲染器 */
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(window.devicePixelRatio);
document.body.append(renderer.domElement);

/* 5、创建控制器 */
const controls = new OrbitControls(camera, renderer.domElement);

/* 6、创建辅助器 */
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

function render() {
  renderer.render(scene, camera);
  requestAnimationFrame(render);
}

render();
</script>
