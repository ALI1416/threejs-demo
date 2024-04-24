2
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
/* 第一个物体 */
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1)
const textureLoader = new THREE.TextureLoader()
const doorColorTexture = textureLoader.load('./textures/door/color.jpg')
const cubeMaterial = new THREE.MeshBasicMaterial({
  map: doorColorTexture,
})
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)
scene.add(cube)

/* 第二个物体 */
const cubeGeometry2 = new THREE.BoxGeometry(1, 1, 1)
const doorAlphaTexture = textureLoader.load('./textures/door/alpha.jpg')
const cubeMaterial2 = new THREE.MeshBasicMaterial({
  map: doorColorTexture,
  // 透明材质贴图 http://localhost:8080/docs/#api/zh/materials/MeshBasicMaterial.alphaMap
  alphaMap: doorAlphaTexture,
  // 开启材质透明选项 http://localhost:8080/docs/#api/zh/materials/Material.transparent
  transparent: true
})
const cube2 = new THREE.Mesh(cubeGeometry2, cubeMaterial2)
cube2.position.set(2, 0, 0)
scene.add(cube2)

/* 第三个物体 */
// 创建一个平面
const planeGeometry = new THREE.PlaneGeometry(1, 1)
const planeMaterial = new THREE.MeshBasicMaterial({
  map: doorColorTexture,
  // 透明度(0-1) http://localhost:8080/docs/#api/zh/materials/Material.opacity
  opacity: 0.5,
  transparent: true,
  // 渲染面 http://localhost:8080/docs/#api/zh/materials/Material.side
  // 默认 THREE.FrontSide 正面
  // 背面 THREE.BackSide
  // 双面 THREE.DoubleSide
  side: THREE.DoubleSide
})
const plane = new THREE.Mesh(planeGeometry, planeMaterial)
plane.position.set(4, 0, 0)
scene.add(plane)

/* 第四个物体 */
const cubeGeometry4 = new THREE.BoxGeometry(1, 1, 1)
const doorAoTexture = textureLoader.load('./textures/door/ambientOcclusion.jpg')
const cubeMaterial4 = new THREE.MeshBasicMaterial({
  map: doorColorTexture,
  alphaMap: doorAlphaTexture,
  transparent: true,
  // 环境遮挡材质贴图(变暗) http://localhost:8080/docs/#api/zh/materials/MeshBasicMaterial.aoMap
  aoMap: doorAoTexture,
  // 环境遮挡强度(0-1) http://localhost:8080/docs/#api/zh/materials/MeshBasicMaterial.aoMapIntensity
  aoMapIntensity: 0.5
})
// 环境遮挡需要设置第二组uv
cubeGeometry4.setAttribute('uv2', new THREE.BufferAttribute(cubeGeometry4.attributes.uv.array, 2))
const cube4 = new THREE.Mesh(cubeGeometry4, cubeMaterial4)
cube4.position.set(0, 0, 2)
scene.add(cube4)

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
