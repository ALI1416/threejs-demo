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
import {MTLLoader, OBJLoader, OrbitControls} from 'three/addons'
// 导入three.js提供的库
// 也可以引用官方库 npm install stats.js
// 官网 https://github.com/mrdoob/stats.js
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
// 本模型 https://3dwarehouse.sketchup.com/model/429ed2c114d5241d465e08d496c0420f/tree
const loader = new OBJLoader()
const mtlLoader = new MTLLoader()
mtlLoader.load('obj/tree.mtl',
    function (mtl) {
      mtl.preload()
      loader.setMaterials(mtl)
      loader.load(
          'obj/tree.obj',
          function (obj) {
            scene.add(obj)
          },
          function (e) {
            console.log('模型已加载' + (e.loaded / e.total * 100) + '%')
          },
          function (e) {
            console.log('模型加载错误', e)
          }
      )
    },
    function (e) {
      console.log('纹理已加载' + (e.loaded / e.total * 100) + '%')
    },
    function (e) {
      console.log('纹理加载错误', e)
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
