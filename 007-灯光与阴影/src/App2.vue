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
import * as dat from "dat.gui"

const gui = new dat.GUI();

/* 1、创建场景 */
const scene = new THREE.Scene();

/* 2、创建相机 */
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.set(0, 0, 10);
scene.add(camera);

/* 3、创建物体 */
const material = new THREE.MeshStandardMaterial();
const sphereGeometry = new THREE.SphereGeometry(1, 20, 20);
const sphere = new THREE.Mesh(sphereGeometry, material);
sphere.castShadow = true;
scene.add(sphere);
const planeGeometry = new THREE.PlaneGeometry(100, 100);
const plane = new THREE.Mesh(planeGeometry, material);
plane.position.set(-20, -1, -20);
plane.rotation.x = -Math.PI / 2;
plane.receiveShadow = true;
scene.add(plane);

/* 光照 */
const light = new THREE.AmbientLight(0xffffff, 0.2);
scene.add(light);
// 聚光灯 http://localhost:8080/docs/#api/zh/lights/SpotLight
const spotLight = new THREE.SpotLight(0xffffff, 0.8);
spotLight.position.set(5, 5, 5);
spotLight.castShadow = true;
spotLight.shadow.radius = 20;
spotLight.shadow.mapSize.set(4096, 4096);
scene.add(spotLight);

const sphereFolder = gui.addFolder("球");
sphereFolder.open();
sphereFolder.add(sphere.position, "x").min(-50).max(5).step(0.1).name("横");
sphereFolder.add(sphere.position, "z").min(-50).max(5).step(0.1).name("竖");
sphereFolder.add(sphere.position, "y").min(0).max(10).step(0.1).name("高");
const spotLightFolder = gui.addFolder("聚光灯");
spotLightFolder.open();
spotLightFolder.add(spotLight.position, "x").min(0).max(10).step(0.1).name("横");
spotLightFolder.add(spotLight.position, "z").min(0).max(10).step(0.1).name("竖");
spotLightFolder.add(spotLight.position, "y").min(0).max(10).step(0.1).name("高");
const params = {
  intensity: 1,
  target: false,
  physicallyCorrectLights: false,
}
// 元素参数
const origin = {
  target: spotLight.target
}
// 光照强度 http://localhost:8080/docs/#api/zh/lights/SpotLight.intensity
spotLightFolder.add(params, "intensity").name("光照强度").min(0).max(10).step(0.1).onChange((v) => {
  spotLight.intensity = v;
})
// 锁定目标 http://localhost:8080/docs/#api/zh/lights/SpotLight.target
spotLightFolder.add(params, "target").name("锁定目标").onChange((v) => {
  if (v) {
    spotLight.target = sphere;
  } else {
    spotLight.target = origin.target;
  }
})
// 角度 http://localhost:8080/docs/#api/zh/lights/SpotLight.angle
spotLightFolder.add(spotLight, "angle").min(0).max(Math.PI / 2).step(0.01).name("角度");
// 距离衰减 http://localhost:8080/docs/#api/zh/lights/SpotLight.distance
spotLightFolder.add(spotLight, "distance").min(0).max(100).step(0.1).name("距离衰减");
// 光锥衰减 http://localhost:8080/docs/#api/zh/lights/SpotLight.penumbra
spotLightFolder.add(spotLight, "penumbra").min(0).max(1).step(0.001).name("光锥衰减");

/* 4、创建渲染器 */
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(window.devicePixelRatio);
renderer.shadowMap.enabled = true;
document.body.append(renderer.domElement);
const correctFolder = gui.addFolder("正确光照模式");
correctFolder.open();
// 正确光照模式 http://localhost:8080/docs/#api/zh/renderers/WebGLRenderer.physicallyCorrectLights
correctFolder.add(renderer, "physicallyCorrectLights").name("正确光照模式");
// 正确距离衰减 http://localhost:8080/docs/#api/zh/lights/SpotLight.decay
// 距离衰减大于0时才有效
correctFolder.add(spotLight, "decay").min(0).max(5).step(0.01).name("正确距离衰减");
// 光功率 http://localhost:8080/docs/#api/zh/lights/SpotLight.power
correctFolder.add(spotLight, "power").min(0).max(spotLight.intensity * Math.PI * 10).step(0.01).name("光功率");

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
