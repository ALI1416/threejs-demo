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
/* 第一个物体 */
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const textureLoader = new THREE.TextureLoader();
const doorColorTexture = textureLoader.load("./textures/door/color.jpg");
const doorAlphaTexture = textureLoader.load("./textures/door/alpha.jpg");
const doorAoTexture = textureLoader.load("./textures/door/ambientOcclusion.jpg");
// 标准网格材质 http://localhost:8080/docs/#api/zh/materials/MeshStandardMaterial
const cubeMaterial = new THREE.MeshStandardMaterial({
  map: doorColorTexture,
  alphaMap: doorAlphaTexture,
  transparent: true,
  aoMap: doorAoTexture,
  aoMapIntensity: 0.8
});
cubeGeometry.setAttribute("uv2", new THREE.BufferAttribute(cubeGeometry.attributes.uv.array, 2));
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
scene.add(cube);

/* 第二个物体 */
// 高度贴图需要设置分段数
const cubeGeometry2 = new THREE.BoxGeometry(1, 1, 1, 100, 100, 100);
const doorHeightTexture = textureLoader.load("./textures/door/height.jpg");
const cubeMaterial2 = new THREE.MeshStandardMaterial({
  map: doorColorTexture,
  alphaMap: doorAlphaTexture,
  transparent: true,
  aoMap: doorAoTexture,
  aoMapIntensity: 0.8,
  // 置换(高度)贴图 http://localhost:8080/docs/#api/zh/materials/MeshStandardMaterial.displacementMap
  displacementMap: doorHeightTexture,
  // 高度贴图高度(0-1) http://localhost:8080/docs/#api/zh/materials/MeshStandardMaterial.displacementScale
  displacementScale: 0.05
});
cubeGeometry2.setAttribute("uv2", new THREE.BufferAttribute(cubeGeometry2.attributes.uv.array, 2));
const cube2 = new THREE.Mesh(cubeGeometry2, cubeMaterial2);
cube2.position.set(2, 0, 0);
scene.add(cube2);

/* 第三个物体 */
// 蓝色轴穿过面的右上角和左下角时，缩放，可以看到光源
const planeGeometry = new THREE.PlaneGeometry(1, 1);
const planeMaterial = new THREE.MeshStandardMaterial({
  map: doorColorTexture,
  // 粗糙度(0-1) http://localhost:8080/docs/#api/zh/materials/MeshStandardMaterial.roughness
  roughness: 0
});
const plane = new THREE.Mesh(planeGeometry, planeMaterial);
plane.position.set(1, 1, 1);
scene.add(plane);

/* 第四个物体 */
// 可以看到合页生锈部分不反光(黑色)
const cubeGeometry4 = new THREE.BoxGeometry(1, 1, 1, 100, 100, 100);
const doorRoughnessTexture = textureLoader.load("./textures/door/roughness.jpg");
const cubeMaterial4 = new THREE.MeshStandardMaterial({
  map: doorColorTexture,
  alphaMap: doorAlphaTexture,
  transparent: true,
  aoMap: doorAoTexture,
  aoMapIntensity: 0.8,
  displacementMap: doorHeightTexture,
  displacementScale: 0.05,
  // 粗糙度贴图 http://localhost:8080/docs/#api/zh/materials/MeshStandardMaterial.roughnessMap
  roughnessMap: doorRoughnessTexture
});
cubeGeometry4.setAttribute("uv2", new THREE.BufferAttribute(cubeGeometry4.attributes.uv.array, 2));
const cube4 = new THREE.Mesh(cubeGeometry4, cubeMaterial4);
cube4.position.set(2, 2, 2);
scene.add(cube4);

/* 光照 */
// 环境光(均匀照亮物体，无阴影) http://localhost:8080/docs/#api/zh/lights/AmbientLight
// 构造函数
// 参数名      中文名       默认值
// color      颜色(可选)    0xffffff
// intensity  强度(可选)    1
const light = new THREE.AmbientLight(0xffffff, 0.2);
scene.add(light);
// 平行光(太阳光) http://localhost:8080/docs/#api/zh/lights/DirectionalLight
// 构造函数
// 参数名      中文名       默认值
// color      颜色(可选)    0xffffff
// intensity  强度(可选)    1
const directionalLight = new THREE.DirectionalLight(0xffffff, 0.6);
// 设置光照位置 http://localhost:8080/docs/#api/zh/lights/DirectionalLight.position
directionalLight.position.set(10, 10, 10);
scene.add(directionalLight);

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
