<script setup>
import { onMounted } from 'vue'
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

onMounted(() => {
  // 建立場景
  const scene = new THREE.Scene()

  // 建立相機
  const camera = new THREE.PerspectiveCamera()
  // const camera = new THREE.PerspectiveCamera(30, width / height, 1, 3000)

  // 建立渲染器
  const renderer = new THREE.WebGLRenderer()

  // 建立一個幾何體
  const geometry = new THREE.SphereGeometry(100, 100, 100)

  // 建立一個材質
  const material = new THREE.MeshLambertMaterial({
    color: 0x0000ff, // 材質顏色
    transparent: true, // 開啟透明度
    opacity: 0.8 // 設置透明度
  })

  // 建立一個光源
  const pointLight = new THREE.PointLight(0xffffff, 1.0)

  // 設置光源不隨距離衰減
  pointLight.decay = 0.0

  // 將光源加入到場景
  pointLight.position.set(400, 400, 400)
  scene.add(pointLight)

  // 建立控制器
  const controls = new OrbitControls(camera, renderer.domElement)

  // 建立坐標輔助線
  const axesHelper = new THREE.AxesHelper(150)
  scene.add(axesHelper)

  // 結合幾何體與材質
  const mesh = new THREE.Mesh(geometry, material)

  // 設定物體位置
  mesh.position.set(0, 10, 0)

  // 設定相機位置
  camera.position.set(200, 200, 200)

  // 將物體加入場景
  scene.add(mesh)

  // 相機指向空間中某個位置
  // camera.lookAt(0, 0, 0)

  // 相機指向物體
  camera.lookAt(mesh.position)

  // 將渲染器的dom元素加入到body中
  const width = 800 // 寬度
  const height = 500 // 高度
  renderer.setSize(width, height)
  renderer.render(scene, camera) // 渲染
  document.getElementById('webgl').appendChild(renderer.domElement)

  const animate = () => {
    requestAnimationFrame(animate)
    // 更新控制器
    controls.update()
    renderer.render(scene, camera)
  }

  animate()
})
</script>
<template>
  <div class="about">
    <div id="webgl" style="margin-top: 200px; margin-left: 100px"></div>
  </div>
</template>

<style></style>
