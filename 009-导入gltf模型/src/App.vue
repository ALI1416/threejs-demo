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
import {GLTFLoader, OrbitControls} from 'three/addons'
import Stats from 'three/addons/libs/stats.module.js'

const stats = new Stats()
document.body.appendChild(stats.domElement)

/* 1、创建场景 */
const scene = new THREE.Scene()

/* 2、创建相机 */
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
camera.position.set(0, 0, 10)
scene.add(camera)

/* 3、导入模型 */
const loader = new GLTFLoader()
loader.load(
    'gltf/tree.glb',
    function (gltf) {
      console.log(gltf)
      scene.add(gltf.scene)
    },
    function (e) {
      console.log('模型已加载' + (e.loaded / e.total * 100) + '%')
    },
    function (e) {
      console.log('模型加载错误', e)
    }
)

/* 光照 */
const light = new THREE.AmbientLight(0xFFFFFF, 1)
scene.add(light)
const directionalLight = new THREE.DirectionalLight(0xFFFFFF, 2)
directionalLight.position.set(5, 5, 5)
directionalLight.castShadow = true
directionalLight.shadow.radius = 20
directionalLight.shadow.mapSize.set(4096, 4096)
directionalLight.shadow.camera.near = 9
directionalLight.shadow.camera.far = 500
directionalLight.shadow.camera.top = 5
directionalLight.shadow.camera.bottom = -5
directionalLight.shadow.camera.left = -5
directionalLight.shadow.camera.right = 5
scene.add(directionalLight)

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
  stats.update()
}

render()
</script>
