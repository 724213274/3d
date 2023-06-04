<template>
  <div class="home">
   我是{{name}}
   <br/>
   小明的信息:{{myInfo}}
   <br/>
   age{{age}}
   <button @click="add">+</button>
  </div>
</template>

<script lang="ts">
/* eslint-disable */
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import Stats from 'three/examples/jsm/libs/stats.module'
import { GUI } from 'three/examples/jsm/libs/lil-gui.module.min'
// eslint-disable-next-line
import { defineComponent ,ref,Ref,reactive} from 'vue';
// import {ref} from 'vue'
export default defineComponent({
  name: 'HomeView',
  components: {
  },
  setup() {
    interface MyObject {
      name: string;
      age: number;
    }

    let name:Ref<string> = ref('小红');
    let age:Ref<number> = ref(22);
    const myInfo:MyObject = reactive({
      name:'小明',
      age:22
    })
   const add = (): void => {
      age.value++;
    };



// 引入性能监视器
// const stats = new Stats();
// document.body.appendChild(stats.dom)

    // 创建一个三维场景scene
const scene = new THREE.Scene();
const geometry = new THREE.BoxGeometry(100, 100, 100);
const material = new THREE.MeshBasicMaterial({ 
    color: 0x00ffff, 
    transparent:true,
    opacity:0.5
});
// 创建网格模型
const mesh = new THREE.Mesh(geometry, material);
mesh.position.set(0,10,0);
scene.add(mesh);


// for(let i = 0; i<10; i++) {
  
//   for(let j = 0;j<10;j++) {
//     const mesh = new THREE.Mesh(geometry, material);
//     mesh.position.set(i*200,0,j*200);
//     scene.add(mesh);
//   }
// }

/*
  点光源设置
*/
const ponintLight = new THREE.PointLight(0xffffff,1.0)

// 点光源点位置
ponintLight.position.set(-400,-200,-300);
// 点光源添加到场景中
scene.add(ponintLight);

// 可视化点光源
// const pointLightHelper  = new THREE.PointLightHelper(ponintLight,10)
// scene.add(pointLightHelper)

// 添加一个环境光 环境光是没有方向的
const ambient = new THREE.AmbientLight(0xffffff,0.4)
scene.add(ambient)

// 添加一个平行光
const directionalLight = new THREE.DirectionalLight(0xffffff,1.0);
directionalLight.position.set(-50,-100,-60)
scene.add(directionalLight)

// 可视化平行光
// const dirLightHelper = new THREE.DirectionalLightHelper(directionalLight,5,0xffffff)
// scene.add(dirLightHelper)

// 辅助观察的坐标系
const axesHelper = new THREE.AxesHelper(100);
scene.add(axesHelper)

// width和height用来设置渲染后，输出的画布宽高度
// const width = 800;
const width = window.innerWidth;
// const height = 500;
const height = window.innerHeight;

/*
* 透视投影相机设置
*/
//30:视场角度，width/height：Canvas画布宽高比，1:近裁截面，3000:远裁截面
// const camera = new THREE.PerspectiveCamera(30, width / height, 1, 8000);
const camera = new THREE.PerspectiveCamera(60, width / height, 1, 3000);
camera.position.set(800,800,800);
camera.lookAt(0,0,0);

// 创建一个WebGL渲染器
const renderer = new THREE.WebGLRenderer({
  antialias: true
});

renderer.setSize(width, height);//canvas画布宽高
renderer.render(scene, camera);//执行一个渲染操作，类比相机的拍照动作
//把渲染器结果canvas画布，也就是所谓的“照片”，添加到网页页面上
document.body.appendChild(renderer.domElement);

  


// 设备像素比
console.log(window.devicePixelRatio,'设备像素比');
renderer.setPixelRatio(window.devicePixelRatio);



// 设置背景颜色
// renderer.setClearColor(0x444444)


  // 创建一个相机控件对象
  const controls = new OrbitControls(camera,renderer.domElement)

// 渲染循环
const clock = new THREE.Clock();//创建一个时钟对象
function render() {
  // stats.update();
  // const spt = clock.getDelta()*1000 //单位毫秒
  // mesh.rotateY(0.01)
  renderer.render(scene,camera)
  requestAnimationFrame(render)
}
render()



window.onresize = function() {
  // 更新canvas画布尺寸
  renderer.setSize(window.innerWidth,window.innerHeight);
  camera.aspect = window.innerWidth/window.innerHeight
  camera.updateProjectionMatrix()
}


// controls.addEventListener('change',function(){
//   renderer.render(scene,camera)
//   console.log(camera.position);
// })




  return {
      age,
      name,
      myInfo,
      add
    }
  }
});

</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
}
body {
  overflow: hidden;
}
</style>