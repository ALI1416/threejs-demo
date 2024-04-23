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
// 安装 npm install lil-gui
// 导入lil-gui
import GUI from 'lil-gui'

/* 1、创建场景 */
const scene = new THREE.Scene()

/* 2、创建相机 */
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
camera.position.set(0, 0, 10)
scene.add(camera)

/* 3、创建物体 */
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1)
const cubeMaterial = new THREE.MeshBasicMaterial({color: 0x66ccff})
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)
scene.add(cube)

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

/* 7、lil-gui动画 */
// 文档 https://lil-gui.georgealways.com/
const gui = new GUI()
// GUI方法 https://lil-gui.georgealways.com/#GUI
// 方法名  中文名
// title  设置标题名
// add    添加控制器
gui.title('控制')

// 控制器方法 https://lil-gui.georgealways.com/#Controller
// 方法名  中文名
// name     设置名称
// onChange 发生改变
// onFinishChange 结束改变
// 7.1、add() https://lil-gui.georgealways.com/#GUI#add
// 构造函数
// 参数名    中文名
// object   目标对象
// property 属性
// min      最小值(可选)
// max      最大值(可选)
// step     步长(可选)
gui.add(cube.position, 'x', 0, 5, 0.1)
    .name('X坐标')
    .onChange((value) => {
      console.log('X发生改变：' + value)
    })
    .onFinishChange((value) => {
      console.log('X结束改变：' + value)
    })

// 自定义参数
const params = {
  // 物体颜色
  color: '#66CCFF',
  // 设置物体颜色函数
  setColor: (color) => {
    cube.material.color.set(color)
  },
  // 设置物体颜色为红色函数
  setColorRed: () => {
    cube.material.color.set('#FF0000')
  }
}

// 7.2、addColor() https://lil-gui.georgealways.com/#GUI#addColor
// 构造函数
// 参数名    中文名
// object   目标对象
// property 属性
gui.addColor(params, 'color').name('颜色').onChange((value) => {
  cube.material.color.set(value)
})
// 添加函数
gui.add(params, 'setColorRed').name('设置颜色为红色')

// 7.3、新增文件夹
const folder = gui.addFolder('其他属性')
// 默认展开
folder.open()
// 查看对象所有属性
console.log(cube)
// 设置对象的其他属性
folder.add(cube, 'visible').name('是否显示物体')
folder.add(cube.material, 'wireframe').name('是否显示骨架')

function render() {
  requestAnimationFrame(render)
  renderer.render(scene, camera)
}

render()
</script>
