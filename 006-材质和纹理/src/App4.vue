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
/* 加载管理器 */
// http://localhost:8080/docs/#api/zh/loaders/managers/LoadingManager
const loadingManager = new THREE.LoadingManager();
// 开始加载 http://localhost:8080/docs/#api/zh/loaders/managers/LoadingManager.onStart
loadingManager.onStart = function (url, itemsLoaded, itemsTotal) {
  console.log("开始加载，地址：" + url + "，已加载个数：" + itemsLoaded + "，总个数：" + itemsTotal);
};
// 全部加载完成 http://localhost:8080/docs/#api/zh/loaders/managers/LoadingManager.onLoad
loadingManager.onLoad = function () {
  console.log("全部加载完成");
};
// 加载进度 http://localhost:8080/docs/#api/zh/loaders/managers/LoadingManager.onProgress
loadingManager.onProgress = function (url, itemsLoaded, itemsTotal) {
  console.log("加载进度，地址：" + url + "，已加载个数：" + itemsLoaded + "，总个数：" + itemsTotal);
};
// 加载出错 http://localhost:8080/docs/#api/zh/loaders/managers/LoadingManager.onError
loadingManager.onError = function (url) {
  console.log("加载出错，地址：" + url);
};

/* 第一个物体 */
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1, 100, 100, 100);
// 把加载管理器传入纹理加载器：TextureLoader被LoadingManager所管理
const textureLoader = new THREE.TextureLoader(loadingManager);
// 加载纹理 http://localhost:8080/docs/#api/zh/loaders/TextureLoader.load
const doorColorTexture = textureLoader.load(
    "./textures/door/color.jpg",
    () => {
      console.log("color加载完成");
    },
    (total, loaded) => {
      console.log("color加载中，总共" + total + "字节，已完成" + loaded + "字节");
    },
    (e) => {
      console.log("color加载出错，原因：");
      console.log(e);
    }
);
const doorAlphaTexture = textureLoader.load("./textures/door/alpha.jpg");
const doorAoTexture = textureLoader.load("./textures/door/ambientOcclusion.jpg");
const doorHeightTexture = textureLoader.load("./textures/door/height.jpg");
const doorRoughnessTexture = textureLoader.load("./textures/door/roughness.jpg");
const doorMetalnessTexture = textureLoader.load("./textures/door/metalness.jpg");
const doorNormalTexture = textureLoader.load("./textures/door/normal.jpg");
const cubeMaterial = new THREE.MeshStandardMaterial({
  map: doorColorTexture,
  alphaMap: doorAlphaTexture,
  transparent: true,
  aoMap: doorAoTexture,
  aoMapIntensity: 0.8,
  displacementMap: doorHeightTexture,
  displacementScale: 0.05,
  roughnessMap: doorRoughnessTexture,
  metalness: 1,
  // 金属度需要设置为1(默认为0)
  // 金属度贴图 http://localhost:8080/docs/#api/zh/materials/MeshStandardMaterial.metalnessMap
  // 可以看到：没有光照时，金属为黑色
  metalnessMap: doorMetalnessTexture,
  // 法线贴图 http://localhost:8080/docs/#api/zh/materials/MeshStandardMaterial.normalMap
  // 可以看到：不同方向边缘不一样
  normalMap: doorNormalTexture
});
cubeGeometry.setAttribute("uv2", new THREE.BufferAttribute(cubeGeometry.attributes.uv.array, 2));
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
scene.add(cube);

/* 第二个物体 */
// 蓝色轴穿过面的右上角和左下角时，缩放，可以看到光源
const planeGeometry = new THREE.PlaneGeometry(1, 1, 100, 100);
const planeMaterial = new THREE.MeshStandardMaterial({
  map: doorColorTexture,
  roughness: 0.1,
  // 只有平滑才有效(roughness<1)
  // 金属度(0-1) http://localhost:8080/docs/#api/zh/materials/MeshStandardMaterial.metalness
  metalness: 0.8
});
const plane = new THREE.Mesh(planeGeometry, planeMaterial);
plane.position.set(1, 1, 1);
scene.add(plane);

/* 光照 */
const light = new THREE.AmbientLight(0xffffff, 0.2);
scene.add(light);
const directionalLight = new THREE.DirectionalLight(0xffffff, 0.6);
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
