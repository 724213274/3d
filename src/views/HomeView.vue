<template>
  <div class="home">
   我是{{name}}
   <br/>
   小明的信息:{{myInfo}}
   <br/>
   age{{age}}
   <button @click="add">+</button>
    <div>
    <a-button type="primary" size="default">Button</a-button>
  </div>
  </div>
</template>

<script lang="ts">
/* eslint-disable */
import * as THREE from 'three';
/* eslint-disable */
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
// eslint-disable-next-line
import Stats from 'three/examples/jsm/libs/stats.module'
/* eslint-disable */
import { GUI } from 'three/examples/jsm/libs/lil-gui.module.min'
/* eslint-disable */
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

    interface MyObj {
      color: number;
      specular: number;
      bool?: boolean;
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



//场景
const scene = new THREE.Scene();


// 一个网格模型
const geometry = new THREE.SphereGeometry(50, 25, 25);
const material = new THREE.MeshPhongMaterial({
    color: 0x00ffff,
    specular: 0x111111,
});
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh); //模型对象添加到场景中

//光源设置
const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
directionalLight.position.set(400, 200, 300);
scene.add(directionalLight);
const ambient = new THREE.AmbientLight(0xffffff, 0.4);
scene.add(ambient);

//辅助观察的坐标系
const axesHelper = new THREE.AxesHelper(100);
scene.add(axesHelper);

const gui = new GUI(); //创建GUI对象 
//创建一个对象，对象属性的值可以被GUI库创建的交互界面改变
const obj:MyObj = {
    color: 0x00ffff,// 材质颜色
    specular: 0x111111,// 材质高光颜色
};
// 创建材质子菜单
const matFolder = gui.addFolder('材质');
// 材质颜色color
matFolder.addColor(obj, 'color').onChange(function(value:string):void{
    material.color.set(value);
});
// 材质高光颜色specular
matFolder.addColor(obj, 'specular').onChange(function(value:string):void{
    material.specular.set(value);
});
// 环境光子菜单
const ambientFolder = gui.addFolder('环境光');
// 环境光强度
ambientFolder.add(ambient, 'intensity',0,2);
// 平行光子菜单
const dirFolder = gui.addFolder('平行光');
// 平行光强度
dirFolder.add(directionalLight, 'intensity',0,2);
// 平行光位置
dirFolder.add(directionalLight.position, 'x',-400,400);
dirFolder.add(directionalLight.position, 'y',-400,400);
dirFolder.add(directionalLight.position, 'z',-400,400);

//渲染器和相机
const width = window.innerWidth;
const height = window.innerHeight;
const camera = new THREE.PerspectiveCamera(30, width / height, 1, 3000);
camera.position.set(292, 223, 185);
camera.lookAt(0, 0, 0);

const renderer = new THREE.WebGLRenderer();
renderer.setSize(width, height);
document.body.appendChild(renderer.domElement);



// 渲染循环
function render() {
    // 当gui界面设置obj.bool为true,mesh执行旋转动画
    if (obj.bool) mesh.rotateY(0.01);
    renderer.render(scene, camera);
    requestAnimationFrame(render);
}
render();


const controls = new OrbitControls(camera, renderer.domElement);

// 画布跟随窗口变化
window.onresize = function () {
    renderer.setSize(window.innerWidth, window.innerHeight);
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
};



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