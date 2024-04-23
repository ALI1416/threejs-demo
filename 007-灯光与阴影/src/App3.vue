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
const material = new THREE.MeshStandardMaterial()
const sphereGeometry = new THREE.SphereGeometry(1, 20, 20)
const sphere = new THREE.Mesh(sphereGeometry, material)
sphere.castShadow = true
scene.add(sphere)
const planeGeometry = new THREE.PlaneGeometry(100, 100)
const plane = new THREE.Mesh(planeGeometry, material)
plane.position.set(-20, -1, -20)
plane.rotation.x = -Math.PI / 2
plane.receiveShadow = true
scene.add(plane)

/* 光照 */
const light = new THREE.AmbientLight(0xFFFFFF, 0.2)
scene.add(light)
// 点光源 http://localhost:8080/docs/#api/zh/lights/PointLight
const pointLight = new THREE.PointLight(0xFF0000, 0.8)
pointLight.castShadow = true
pointLight.shadow.radius = 20

/* 点光源小球 */
const ball = new THREE.Mesh(new THREE.SphereGeometry(0.1), new THREE.MeshBasicMaterial({color: 0xFF0000}))
ball.position.set(2, 2, 2)
// 小球绑定光源
ball.add(pointLight)
// 场景加入小球
scene.add(ball)

const pointLightFolder = gui.addFolder('点光源')
pointLightFolder.open()
pointLightFolder.add(ball.position, 'x').min(-10).max(10).step(0.1).name('横')
pointLightFolder.add(ball.position, 'z').min(-10).max(10).step(0.1).name('竖')
pointLightFolder.add(ball.position, 'y').min(0).max(10).step(0.1).name('高')
const params = {
  intensity: 1,
  surround: false,
  surroundRadius: 2,
}
// 光照强度 http://localhost:8080/docs/#api/zh/lights/PointLight.intensity
pointLightFolder.add(params, 'intensity').name('光照强度').min(0).max(10).step(0.1).onChange((v) => {
  pointLight.intensity = v
})
// 距离衰减 http://localhost:8080/docs/#api/zh/lights/PointLight.distance
pointLightFolder.add(pointLight, 'distance').min(0).max(100).step(0.1).name('距离衰减')
// 光功率 http://localhost:8080/docs/#api/zh/lights/PointLight.power
pointLightFolder.add(pointLight, 'power').min(0).max(pointLight.intensity * Math.PI * 10).step(0.01).name('光功率')
// 正确距离衰减 http://localhost:8080/docs/#api/zh/lights/PointLight.decay
// 距离衰减大于0时才有效
pointLightFolder.add(pointLight, 'decay').min(0).max(5).step(0.01).name('正确距离衰减')

/* 4、创建渲染器 */
const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)
renderer.setPixelRatio(window.devicePixelRatio)
renderer.shadowMap.enabled = true
document.body.append(renderer.domElement)

/* 5、创建控制器 */
const controls = new OrbitControls(camera, renderer.domElement)

/* 6、创建辅助器 */
const axesHelper = new THREE.AxesHelper(5)
scene.add(axesHelper)

/* 时钟 */
const clock = new THREE.Clock()
const surroundFolder = gui.addFolder('环绕模式')
surroundFolder.open()
surroundFolder.add(params, 'surround').name('开启环绕')
surroundFolder.add(params, 'surroundRadius').name('环绕半径').min(0).max(10).step(0.1)

function render() {
  requestAnimationFrame(render)
  if (params.surround) {
    let time = clock.getElapsedTime()
    ball.position.x = Math.sin(time) * params.surroundRadius
    ball.position.z = Math.cos(time) * params.surroundRadius
  }
  renderer.render(scene, camera)
}

render()
</script>
