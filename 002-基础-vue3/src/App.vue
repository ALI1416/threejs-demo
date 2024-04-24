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
// 安装 npm install three
// 导入整个three.js核心库
import * as THREE from 'three'
// 5.0、导入轨道控制器依赖
import {OrbitControls} from 'three/addons'

/* 1、创建场景 */
const scene = new THREE.Scene()

/* 2、创建相机 */
// 2.1、创建透视相机 http://localhost:8080/docs/index.html#api/zh/cameras/PerspectiveCamera
// 构造函数
// 参数名  中文名   推荐值
// fov    可视角度  75
// aspect 宽高比   window.innerWidth / window.innerHeight(屏幕宽高比)
// near   近端距离  0.1
// far    远端距离  1000
// 视椎体案例 http://localhost:8080/examples/#webgl_camera
// 黄色是可视区域，红色离得太近无法看到，其他区域超出角度或太远无法看到
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
// 2.2、设置相机位置
// PerspectiveCamera继承于Camera继承于Object3D
// 存在position(位置)属性 http://localhost:8080/docs/index.html#api/zh/core/Object3D.position
// 它是Vector3(三维向量)对象 http://localhost:8080/docs/index.html#api/zh/math/Vector3
// 使用set方法设置xyz坐标 http://localhost:8080/docs/index.html#api/zh/math/Vector3.set
camera.position.set(0, 0, 10)
// 2.3、把相机添加到场景中
scene.add(camera)

/* 3、创建物体 */
// 3.1、创建立方缓冲几何体 http://localhost:8080/docs/#api/zh/geometries/BoxGeometry
// 构造函数
// 参数名  中文名   默认值
// width  宽(X)  1
// height 高(Y)  1
// depth  深(Z)  1
// widthSegments  宽度的分段数(可选)  1
// heightSegments 高度的分段数(可选)  1
// depthSegments  深度的分段数(可选)  1
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1)
// 3.2、设置基础网格材质 http://localhost:8080/docs/#api/zh/materials/MeshBasicMaterial
// 属性
// 参数名  中文名   默认值
// color  颜色      0xffffff
const cubeMaterial = new THREE.MeshBasicMaterial({color: 0x66CCFF})
// 3.3、根据几何体和材质创建物体 http://localhost:8080/docs/#api/zh/objects/Mesh
// 构造函数
// 参数名    中文名   默认值
// geometry 物体      BufferGeometry
// material 材质或数组 MeshBasicMaterial
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)
// 3.4、把物体添加到场景中
scene.add(cube)

/* 4、创建渲染器 */
// 4.1、创建WebGL渲染器 http://localhost:8080/docs/#api/zh/renderers/WebGLRenderer
const renderer = new THREE.WebGLRenderer()
// 4.2、设置渲染器大小 http://localhost:8080/docs/#api/zh/renderers/WebGLRenderer.setSize
// 构造函数
// 参数名  中文名   推荐值
// width  宽       window.innerWidth
// height 高       window.innerHeight
// updateStyle ?  ?
renderer.setSize(window.innerWidth, window.innerHeight)
// 4.3、设置渲染器像素比 http://localhost:8080/docs/#api/zh/renderers/WebGLRenderer.setPixelRatio
// 构造函数
// 参数名  中文名   推荐值
// value  像素比   window.devicePixelRatio
renderer.setPixelRatio(window.devicePixelRatio)
// 4.4、将WebGL渲染的canvas内容添加到body
document.body.append(renderer.domElement)
// 4.5、使用渲染器，通过相机讲场景渲染出来
// 构造函数
// 参数名  中文名
// scene  场景
// camera 相机
// 注：在5.2中每一帧刷新
// renderer.render(scene, camera);

/* 5、创建控制器 */
// 5.1、创建轨道控制器 http://localhost:8080/docs/#examples/zh/controls/OrbitControls
// 构造函数
// 参数名    中文名
// camera     相机
// domElement 渲染的canvas
const controls = new OrbitControls(camera, renderer.domElement)

/* 6、创建辅助器 */
// 6.1、创建坐标辅助器 http://localhost:8080/docs/index.html#api/zh/helpers/AxesHelper
// 红色代表X轴、绿色代表Y轴、蓝色代表Z轴
// 构造函数
// 参数名    中文名     默认值
// size     长度(可选)  1
const axesHelper = new THREE.AxesHelper(5)
// 6.2、把坐标辅助器添加到场景中
scene.add(axesHelper)

// 5.2、每一帧刷新
function render() {
  // 渲染下一帧
  requestAnimationFrame(render)
  renderer.render(scene, camera)
}

// 5.3、调用渲染方法
render()
</script>
