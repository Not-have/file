# 一、threejs文档

中文文档：http://www.yanhuangxueyuan.com/threejs/docs/index.html

​				   http://www.yanhuangxueyuan.com/Three.js/

官方文档（推荐）：https://threejs.org/

​                   https://threejs.org/docs/index.html#manual/zh/introduction/Creating-a-scene

threejs是基于WebGL封装的

# 二、下载

```bash
# 安装整个threejs
npm install --save three
```

安装的具体，请查看下面的文档：

https://threejs.org/docs/index.html#manual/zh/introduction/Installation

# 三、使用



# 四、使用过程中遇到的问题

## 1、处理threejs *OrbitControls*  方法点击时出现的边框

```javascript
/**
  * 去除鼠标旋转时，预留的边框
  */
renderer.domElement.removeAttribute("tabindex")
```

![image-20211213181229076](https://gitee.com/Green_chicken/picture/raw/master/markdown/image-20211213181229076.png)

```javascript
<template>
    <div ref="demo1" class="demo1"></div>
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";
import * as THREE from "three";
import OrbitControls from 'three-orbitcontrols';
export default {
    name: "demo1",
    components: {},
    setup() {
        let demo1 = ref(null);
        onMounted(() => {
            let el = demo1.value;
            /**
             * 创建场景对象Scene
             */
            var scene = new THREE.Scene();
            /**
             * 创建网格模型
             */
            // var geometry = new THREE.SphereGeometry(60, 40, 40); //创建一个球体几何对象
            var geometry = new THREE.BoxGeometry(100, 100, 100); //创建一个立方体几何对象Geometry
            var material = new THREE.MeshLambertMaterial({
                color: 0x5f9ea0,
                lightMapIntensity: 3,
            }); //材质对象Material
            var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
            scene.add(mesh); //网格模型添加到场景中
            /**
             * 光源设置
             */
            //点光源
            var point = new THREE.PointLight(0xffffff);
            point.position.set(400, 600, 600); //点光源位置
            scene.add(point); //点光源添加到场景中
            //环境光
            var ambient = new THREE.AmbientLight(0x444444);
            scene.add(ambient);
            // console.log(scene)
            // console.log(scene.children)
            /**
             * 相机设置
             */
            var width = demo1.value.offsetWidth; //窗口宽度
            var height = demo1.value.offsetHeight; //窗口高度
            var k = width / height; //窗口宽高比
            var s = 400; //三维场景显示范围控制系数，系数越大，显示的范围越大
            //创建相机对象
            var camera = new THREE.OrthographicCamera(
                -s * k,
                s * k,
                s,
                -s,
                1,
                1000
            );
            camera.position.set(200, 300, 200); //设置相机位置
            camera.lookAt(scene.position); //设置相机方向(指向的场景对象)
            /**
             * 创建渲染器对象
             */
            var renderer = new THREE.WebGLRenderer();
            renderer.setSize(width, height); //设置渲染区域尺寸
            renderer.setClearColor(0xffffff, 1); //设置背景颜色
            el.appendChild(renderer.domElement); //body元素中插入canvas对象
            //执行渲染操作   指定场景、相机作为参数
            // renderer.render(scene, camera);
            var controls = new OrbitControls(camera, renderer.domElement);//创建控件对象
            /**
             * 去除鼠标旋转时，预留的边框
             */
            renderer.domElement.removeAttribute("tabindex")
            controls.addEventListener('change', render);//监听鼠标、键盘事件

            let T0 = new Date(); //上次时间
            function render() {
                let T1 = new Date(); //本次时间
                let t = T1 - T0; //时间差
                T0 = T1; //把本次时间赋值给上次时间
                requestAnimationFrame(render);
                renderer.render(scene, camera); //执行渲染操作
                mesh.rotateY(.00010 * t); //旋转角速度0.001弧度每毫秒
            }
            render();
        });
        return { demo1 };
    },
}
</script>

<style scoped>
.demo1 {
    width: 100%;
    height: 100%;
}
</style>
```

 ![chrome-capture](https://gitee.com/Green_chicken/picture/raw/master/markdown/chrome-capture.gif)

## 2、