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
import {OrbitControls} from 'three/addons/controls/OrbitControls.js'
import GUI from 'lil-gui'

const gui = new GUI()

/* 1、创建场景 */
const scene = new THREE.Scene()

/* 2、创建相机 */
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
camera.position.set(0, 0, 10)
scene.add(camera)

/* 3、创建物体 */
// 球
const sphereGeometry = new THREE.SphereGeometry(1, 20, 20)
const sphereMaterial = new THREE.MeshStandardMaterial()
const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial)
// 3、物体投射阴影 http://localhost:8080/docs/#api/zh/core/Object3D.castShadow
sphere.castShadow = true
scene.add(sphere)
// 平面
const planeGeometry = new THREE.PlaneGeometry(10, 10)
const planeMaterial = new THREE.MeshStandardMaterial()
const plane = new THREE.Mesh(planeGeometry, planeMaterial)
plane.position.set(0, -1, 0)
plane.rotation.x = -Math.PI / 2
// 4、物体接收阴影 http://localhost:8080/docs/#api/zh/core/Object3D.receiveShadow
plane.receiveShadow = true
scene.add(plane)

/* 光照 */
const light = new THREE.AmbientLight(0xFFFFFF, 0.2)
scene.add(light)
const directionalLight = new THREE.DirectionalLight(0xFFFFFF, 0.6)
directionalLight.position.set(5, 5, 5)
// 2、光照投射阴影 http://localhost:8080/docs/#api/zh/lights/DirectionalLight.castShadow
directionalLight.castShadow = true
// 阴影贴图设置
// 模糊度 http://localhost:8080/docs/#api/zh/lights/shadows/LightShadow.radius
directionalLight.shadow.radius = 20
// 贴图分辨率 http://localhost:8080/docs/#api/zh/lights/shadows/LightShadow.mapSize
directionalLight.shadow.mapSize.set(4096, 4096)
// 平行光投射相机属性(近远上下左右)[节省计算量] http://localhost:8080/docs/#api/zh/lights/shadows/LightShadow.camera
directionalLight.shadow.camera.near = 9
directionalLight.shadow.camera.far = 500
directionalLight.shadow.camera.top = 5
directionalLight.shadow.camera.bottom = -5
directionalLight.shadow.camera.left = -5
directionalLight.shadow.camera.right = 5
scene.add(directionalLight)

// 调节相机近端
gui.add(directionalLight.shadow.camera, 'near').min(9).max(10).step(0.01).name('近端').onChange(() => {
  // 调节后要更新相机矩阵
  directionalLight.shadow.camera.updateProjectionMatrix()
})

/* 4、创建渲染器 */
const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)
renderer.setPixelRatio(window.devicePixelRatio)
// 1、渲染器渲染阴影 http://localhost:8080/docs/#api/zh/renderers/WebGLRenderer.shadowMap
renderer.shadowMap.enabled = true
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
