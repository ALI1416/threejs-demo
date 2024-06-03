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
import Stats from 'three/addons/libs/stats.module.js'

const stats = new Stats()
document.body.appendChild(stats.domElement)

/* 1、创建场景 */
const scene = new THREE.Scene()

/* 2、创建相机 */
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
camera.position.set(0, 100, 0)
scene.add(camera)

/* 3、创建管道 */
const points = [
  [42, 0, 10],
  [21, 0, 10],
  [21, 0, 1],
  [-3, 0, 1],
  [-3, 0, -18],
  [-10, 0, -18],
  [-10, 0, 5],
  [1, 0, 5],
  [1, 0, 24],
  [-27, 0, 24],
  [-27, 0, 18],
  [-46, 0, 19],
  [-46, 0, -4],
  [-25, 0, -6],
  [-25, 0, -19],
  [-35, 0, -20],
  [-35, 0, -26],
  [-30, 0, -30],
  [3, 0, -30]
]

function createPath(points) {
  let path = new THREE.CurvePath()
  let pointsVector3 = points.map((point) => new THREE.Vector3(...point))
  for (let i = 0; i < pointsVector3.length - 1; i++) {
    path.curves.push(new THREE.LineCurve3(pointsVector3[i], pointsVector3[i + 1]))
  }
  return path
}

const curve = createPath(points)

const tubeGeometry = new THREE.TubeGeometry(curve, 1000, 1, 10)

const texLoader = new THREE.TextureLoader()
const texture = texLoader.load('arrow-right.png')
texture.wrapS = THREE.RepeatWrapping
texture.wrapT = THREE.RepeatWrapping
texture.repeat.x = 100
texture.repeat.y = 1

const tubeMaterial = new THREE.MeshPhongMaterial({
  color: 0x66ccff,
  map: texture,
  transparent: true,
  side: THREE.DoubleSide,
})

const mesh = new THREE.Mesh(tubeGeometry, tubeMaterial)
scene.add(mesh)

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
  texture.offset.x += 0.01
}

render()
</script>
