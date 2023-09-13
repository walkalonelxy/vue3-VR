<template>
	<div class="container" ref="container"></div>
</template>

<script setup>
  import { ref, onMounted } from 'vue'
  import * as THREE from "three"
  // 导入轨道控制器 - 控制物体的左右上下移动（ 可以设置xyz轴 ）
  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"
  // 初始化场景
  const scene = new THREE.Scene()
  // 初始化相机
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
  // 设置相机位置 x y z
  camera.position.set(0, 0, 10)
  // 把相机添加到场景之中
  scene.add(camera);

  // 立方体
  const geometry = new THREE.BoxGeometry(40, 40, 40)
  // const materials = new THREE.MeshBasicMaterial({color:0x00ff00})
  // const mesh = new THREE.Mesh(geometry,materials)
  // mesh.castShadow = true; // 设置物体投影阴影
  // scene.add(mesh)

  // 立方体纹理加载
  let arr = ['scene_left', 'scene_right', 'scene_top', 'scene_bottom', 'scene_front', 'scene_back'];
  let boxMaterials = []
  arr.forEach(item => {
    const texttrue = new THREE.TextureLoader().load(require(`../assets/img/scene_bottom.jpeg`)) // 纹理贴纸
    // if ( item === 'scene_top' || item === 'scene_bottom' ) {
    //   texttrue.rotation = Math.PI;
    //   texttrue.center = new THREE.Vector2(0.5,0.5) // 旋转中心
    //     // 基础材质
    //   boxMaterials.push(
    //     new THREE.MeshBasicMaterial({
    //       color:'#ffff00',
    //       map:texttrue
    //     })
    //   )
    // } else {
    //     // 基础材质
    boxMaterials.push(
      new THREE.MeshBasicMaterial({
        color: '#ffff00',
        map: texttrue
      })
    )
    // }
  })
  const mesh = new THREE.Mesh(geometry, boxMaterials)
  mesh.geometry.scale(1, 1, -1) // 进入内部
  scene.add(mesh)


  // 设置渲染器
  const renderer = new THREE.WebGL1Renderer()
  // 设置渲染的尺寸大小
  renderer.setSize(window.innerWidth, window.innerHeight)

  // 创建轨道控制器
  const controls = new OrbitControls(camera, renderer.domElement)
  // 06-1：添加坐标轴辅助器
  // const axesHelper = new THREE.AxesHelper( 5 );
  // scene.add( axesHelper );

  // 渲染函数 - 每一帧渲染一次
  function render() {
    // 渲染下一帧 的时候 会调用 animate 函数
    requestAnimationFrame(render);
    renderer.render(scene, camera);
  }

  // 将webgl渲染的canvas内容添加到div上
  const container = ref(null)
  // dom节点挂载之后 渲染dom节点
  onMounted(() => {
    container.value.appendChild(renderer.domElement)
    // 设置控制器阻尼，让控制器更具有真是效果
    controls.enableDamping = true
    render()
  })



</script>

<style lang="scss" scoped>
* {
	padding: 0;
	margin: 0;
}
.container {
	height: 100;
	width: 100%;
	background: "#f0f0f0";
}
</style>
