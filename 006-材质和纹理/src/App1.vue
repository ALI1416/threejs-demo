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
// 纹理下载
// https://www.poliigon.com/
// https://3dtextures.me/
// https://www.arroway-textures.ch/
// https://quixel.com/bridge 安装Bridge使用虚幻引擎账号登录后免费下载
// 纹理加载器 http://localhost:8080/docs/#api/zh/loaders/TextureLoader
const textureLoader = new THREE.TextureLoader()
// 导入纹理 http://localhost:8080/docs/#api/zh/loaders/TextureLoader.load
// 返回Texture对象 http://localhost:8080/docs/#api/zh/textures/Texture
const doorColorTexture = textureLoader.load('./textures/door/color.jpg')
// 设置偏移(Vector2)(0-1) http://localhost:8080/docs/#api/zh/textures/Texture.offset
// 向左偏移20%，向上偏移10%
// doorColorTexture.offset.x = 0.2;
// doorColorTexture.offset.y = -0.1;
// 也可以这样设置
doorColorTexture.offset.set(0.2, -0.1)
// 旋转(弧度制) http://localhost:8080/docs/#api/zh/textures/Texture.rotation
// 旋转10度
doorColorTexture.rotation = Math.PI / 18
// 设置旋转中心点(Vector2) http://localhost:8080/docs/#api/zh/textures/Texture.center
// (0.5, 0.5)对应纹理的正中心
// 默认值为(0, 0)即左下角
doorColorTexture.center.set(0.5, 0.5)
const cubeMaterial = new THREE.MeshBasicMaterial({
  // 颜色材质贴图 http://localhost:8080/docs/#api/zh/materials/MeshBasicMaterial.map
  map: doorColorTexture,
})
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)
scene.add(cube)

/* 第二个物体 */
const cubeGeometry2 = new THREE.BoxGeometry(2, 2, 2)
const doorColorTexture2 = textureLoader.load('./textures/door/color.jpg')
// 重复(Vector2) http://localhost:8080/docs/#api/zh/textures/Texture.repeat
// 水平2次，垂直3次
doorColorTexture2.repeat.set(2, 3)
// 纹理水平包裹 http://localhost:8080/docs/#api/zh/textures/Texture.wrapS
// 纹理垂直包裹 http://localhost:8080/docs/#api/zh/textures/Texture.wrapT
// 默认 THREE.ClampToEdgeWrapping 纹理边缘将被推到外部边缘的纹素
// 重复 THREE.RepeatWrapping
// 镜像重复 THREE.MirroredRepeatWrapping
doorColorTexture2.wrapS = THREE.MirroredRepeatWrapping
doorColorTexture2.wrapT = THREE.RepeatWrapping
const cubeMaterial2 = new THREE.MeshBasicMaterial({
  map: doorColorTexture2,
})
const cube2 = new THREE.Mesh(cubeGeometry2, cubeMaterial2)
cube2.position.set(3, 0, 0)
scene.add(cube2)

/* 第三个物体 */
const cubeGeometry3 = new THREE.BoxGeometry(1, 1, 1)
const doorColorTexture3 = textureLoader.load('./textures/minecraft.png')
// 放大滤镜 http://localhost:8080/docs/#api/zh/textures/Texture.magFilter
// 默认 THREE.LinearFilter 抗锯齿
// 普通 THREE.NearestFilter
doorColorTexture3.magFilter = THREE.NearestFilter
// 缩小滤镜 http://localhost:8080/docs/#api/zh/textures/Texture.minFilter
// THREE.NearestFilter
// THREE.NearestMipmapNearestFilter
// THREE.NearestMipmapLinearFilter
// THREE.LinearFilter
// THREE.LinearMipmapNearestFilter
// 默认 THREE.LinearMipmapLinearFilter
const cubeMaterial3 = new THREE.MeshBasicMaterial({
  map: doorColorTexture3,
})
const cube3 = new THREE.Mesh(cubeGeometry3, cubeMaterial3)
cube3.position.set(0, 0, 2)
scene.add(cube3)

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
