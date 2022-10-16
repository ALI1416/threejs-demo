<template>
</template>

<style>
/*去除白边框*/
body {
  margin: 0;
}
</style>

<script>
/* 0、安装并导入依赖 */
import * as THREE from 'three';
import {OrbitControls} from "three/examples/jsm/controls/OrbitControls.js"
// 安装 npm install gsap
// 导入gsap
import gsap from "gsap";

/* 1、创建场景 */
const scene = new THREE.Scene();

/* 2、创建相机 */
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.set(0, 0, 10);
scene.add(camera);

/* 3、创建物体 */
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({color: 0x66ccff});
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
scene.add(cube);

/* 4、创建渲染器 */
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.append(renderer.domElement);

/* 5、创建控制器 */

const controls = new OrbitControls(camera, renderer.domElement);

/* 6、创建辅助器 */
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

/* 7、gsap动画 */
// cube沿着x轴方向移动3单位，在5s内
// cube是Mesh继承于Object3D有属性position是对象Vector3有属性xyz
// http://localhost:8080/docs/index.html#api/zh/math/Vector3
// ease设置时间曲线 https://greensock.com/get-started#easing
gsap.to(cube.position, {x: 3, ease: "bounce.out", duration: 5})
// 沿着x轴正方向看cube，顺时针旋转一周，在5s内(Math.PI是旋转180度)
// cube是Mesh继承于Object3D有属性rotation是对象Euler有属性xyz和order
// http://localhost:8080/docs/index.html#api/zh/math/Euler
gsap.to(cube.rotation, {x: Math.PI * 2, duration: 5})
// cube的x的值增加到2单位，在5s内
// cube是Mesh继承于Object3D有属性scale是对象Vector3有属性xyz
gsap.to(cube.scale, {x: 2, duration: 5})

function render() {
  renderer.render(scene, camera);
  requestAnimationFrame(render);
}

render();
</script>
