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
// 立方体纹理加载器 http://localhost:8080/docs/#api/zh/loaders/CubeTextureLoader
const streetEnvMap = new THREE.CubeTextureLoader()
    // 设置目录
    .setPath('./textures/environmentMaps/street/')
    // 设置6个面的贴图(p是positive，代表正方向；n是negative，代表负方向；xyz分别是3个轴)
    .load([
      'px.jpg',
      'nx.jpg',
      'py.jpg',
      'ny.jpg',
      'pz.jpg',
      'nz.jpg'
    ]);
// 设置场景的背景 http://localhost:8080/docs/#api/zh/scenes/Scene.background
scene.background = streetEnvMap;
// 设置场景中所有物体的默认环境贴图 http://localhost:8080/docs/?q=sc#api/zh/scenes/Scene.environment
// scene.environment = streetEnvMap;
// 创建一个球
const sphereGeometry = new THREE.SphereBufferGeometry(1, 20, 20);
const sphereMaterial = new THREE.MeshStandardMaterial({
  // 金属度
  metalness: 0.7,
  // 粗糙度
  roughness: 0.1,
  // 环境贴图 http://localhost:8080/docs/#api/zh/materials/MeshStandardMaterial.envMap
  envMap: streetEnvMap
});
const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial);
scene.add(sphere);

/* 光照 */
const light = new THREE.AmbientLight(0xffffff, 0.2);
scene.add(light);
const directionalLight = new THREE.DirectionalLight(0xffffff, 0.6);
directionalLight.position.set(0, 10, -10);
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
