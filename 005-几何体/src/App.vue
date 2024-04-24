<template>
</template>

<style>
body {
  margin: 0;
}
</style>

<script>
/* 0、安装并导入依赖 */
import * as THREE from 'three'
import {OrbitControls} from 'three/addons'

/* 1、创建场景 */
const scene = new THREE.Scene()

/* 2、创建相机 */
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
camera.position.set(0, 0, 10)
scene.add(camera)

/* 3、创建物体 */
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1)
const cubeMaterial = new THREE.MeshBasicMaterial({color: 0x66CCFF})
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)
cube.position.set(3, 0, 0)
scene.add(cube)
// 物体
console.log(cube)
// 几何体 等同于cube.geometry
console.log(cubeGeometry)
// 属性 cubeGeometry.attributes
// BoxGeometry(立方缓冲几何体)继承与BufferGeometry有属性attributes http://localhost:8080/docs/#api/zh/core/BufferGeometry
// 立方体有6个面，每个面有4个顶点，共计6x4=24个顶点
// 属性名    中文名   解释
// normal   法相量   24个顶点三维法向量，每个顶点有3个向量，共计24x3=72个值
// position 坐标    24个顶点三维坐标，每个顶点有3个坐标，共计24x3=72个值
// uv       UV坐标  24个顶点二维展开后坐标，每个顶点有2个坐标，共计24x2=48个值
console.log(cubeGeometry.attributes)

/* 使用BufferGeometry创建几何体 */
// 创建几何体(三角形)
const triangleGeometry = new THREE.BufferGeometry()
// 定义顶点坐标
// 一个面有3个顶点，每个顶点有3个坐标，共计9x3=9个值
const trianglePosition = new Float32Array([
  1.0, 1.0, 1.0, // 右上角
  -1.0, 1.0, 1.0, // 左上角
  -1.0, -1.0, 1.0, // 左下角
])
// 设置顶点坐标
triangleGeometry.setAttribute('position', new THREE.BufferAttribute(trianglePosition, 3))
// 设置基础网格材质
const triangleMaterial = new THREE.MeshBasicMaterial({color: 0xFFFF00})
// 根据几何体和材质创建物体
const triangle = new THREE.Mesh(triangleGeometry, triangleMaterial)
// 把物体添加到场景中
scene.add(triangle)

/* 4、创建渲染器 */
const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)
renderer.setPixelRatio(window.devicePixelRatio)
document.body.append(renderer.domElement)

/* 5、创建控制器 */
const controls = new OrbitControls(camera, renderer.domElement)

/* 6、创建辅助器 */
const axesHelper = new THREE.AxesHelper(5)
scene.add(axesHelper)

function render() {
  requestAnimationFrame(render)
  renderer.render(scene, camera)
}

render()
</script>
