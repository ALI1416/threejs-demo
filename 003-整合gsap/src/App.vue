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
renderer.setPixelRatio(window.devicePixelRatio);
document.body.append(renderer.domElement);

/* 5、创建控制器 */
const controls = new OrbitControls(camera, renderer.domElement);

/* 6、创建辅助器 */
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

/* 7、gsap动画 */
// 文档 https://greensock.com/docs/v3/GSAP
// to() https://greensock.com/docs/v3/GSAP/gsap.to()
// 构造函数
// 参数名    中文名
// targets  目标对象
// vars     参数对象
// vars参数名  中文名     默认值
// duration   持续时间    0.5s
// delay      延迟多少s后执行  0s
// repeat     重复执行多少次   0(-1无限循环)
// yoyo       往复运动(需要和repeat配合使用)    false
// ease       时间曲线    https://greensock.com/get-started#easing
// onStart()  开始执行回调函数
// onComplete()   执行完成回调函数

// 7.1、cube沿着x轴方向移动3单位，在5s内
// cube是Mesh继承于Object3D有属性position是对象Vector3有属性xyz
// http://localhost:8080/docs/index.html#api/zh/math/Vector3
gsap.to(cube.position, {
  x: 3, // 沿着x轴方向移动3单位
  duration: 5, // 在5s内
  ease: "bounce.out", // 时间曲线
  onStart: () => { // 开始执行回调函数
    console.log("开始执行")
  },
  onComplete: () => { // 执行完成回调函数
    console.log("执行完成")
  },
})

// 7.2、沿着x轴正方向看cube，顺时针旋转一周(Math.PI是旋转180度)，在5s内
// cube是Mesh继承于Object3D有属性rotation是对象Euler有属性xyz和order
// http://localhost:8080/docs/index.html#api/zh/math/Euler
let rotateAnimate = gsap.to(cube.rotation, {
  x: Math.PI * 2, // 顺时针旋转一周
  duration: 5, // 在5s内
  repeat: -1, // 无限循环
  yoyo: true, // 往复运动
})

// 7.3、cube的x的值增加到2单位，在5s内
// cube是Mesh继承于Object3D有属性scale是对象Vector3有属性xyz
gsap.to(cube.scale, {
  x: 2, // x的值增加到2单位
  duration: 5, // 在5s内
})

/* 8、监听事件 */
// 8.1、鼠标单击，开始或暂停旋转
window.addEventListener("click", () => {
  // 判断动画是否正在播放
  if (rotateAnimate.isActive()) {
    // 暂停
    rotateAnimate.pause();
  } else {
    // 继续播放
    rotateAnimate.resume()
  }
})

// 8.2、鼠标双击，全屏和退出全屏
window.addEventListener("dblclick", () => {
  // 判断是否全屏
  if (document.fullscreenElement) {
    // 退出全屏
    document.exitFullscreen();
  } else {
    // 动画全屏
    renderer.domElement.requestFullscreen();
  }
})

// 8.3、页面尺寸发生变化，调整画布
window.addEventListener("resize", () => {
  // 更新摄像头宽高比
  camera.aspect = window.innerWidth / window.innerHeight;
  // 更新摄像头的投影矩阵
  camera.updateProjectionMatrix();
  // 更新渲染器尺寸
  renderer.setSize(window.innerWidth, window.innerHeight);
  // 更新渲染器像素比
  renderer.setPixelRatio(window.devicePixelRatio);
})

function render() {
  renderer.render(scene, camera);
  requestAnimationFrame(render);
}

render();
</script>
