# Three.js

[一文带你悟道 Threejs 3D模型开发](https://juejin.cn/post/7170868138068672548)

[Three.js 进阶之旅：模型光源结合生成明暗变化的创意页面-光与影之诗 💡](https://juejin.cn/post/7148969678642102286#heading-27)

[从零开始初尝Three.js【大量案例、简单入手】](https://juejin.cn/post/6844904177345232903)

[Three.js教程 api](http://www.webgl3d.cn/Three.js/?_blank)

[Three.js git hub](https://github.com/mrdoob/three.js/?_blank)

[Three.js官网](https://threejs.org/?_blank)

[Three.js中文文档](http://www.yanhuangxueyuan.com/threejs/docs/index.html?_blank)

[three.js](https://threejs.org/docs/index.html#manual/zh/introduction/Creating-a-scene)

## 加载模型

GLTF OBJ

## 模型网站

[sketchfab](https://www.sketchfab.com/)

[cgmodel](https://www.cgmodel.com/)

[free3D](https://free3d.com/)

等等

## 阴影

1. 设置模型 castShadow receiveShadow.
2. 设置光源的阴影属性

## 纹理贴图

格式：基本图片，立方体纹理, hdr等二进制图，块类的纹理文件

- 凹凸贴图

bumpMap 表面深浅

- 法向贴图

normalMap 表面深浅

- 移位贴图

displacementMap 真正改变模型的形状

- 环境光遮挡贴图

aoMap

静止物体的光照阴影

- 光照贴图

lightMap: 产生假阴影. 只能用于静态场景。

- 金属光泽贴图 和 粗糙度贴图

金属光泽贴图: metalnessMap

粗糙度贴图: roughnessMap

- Alpha贴图

制定表面部分为透明,纯黑色透明，纯白色不透明

alphaMap

- 自发光贴图

控制模型表面实现自发光效果

只影响自身，不会变成光源

自发光: emissiveMap emissive

- 高光贴图

需要使用 MeshPhongMaterial 材质

specularMap

- 环境贴图

cpu真实计算光线反射消耗很大

创建一个对象所处环境纹理来伪装镜面反射。

- cubeMap天空盒纹理

天空盒纹理，设置为场景背景

可以资源网站下载:全景图片, 球形等距圆柱投影图

## 纹理的其他用法

- 自定义uv映射

...

## TextureLoader 纹理加载器

- CubeTextureLoader

立方体纹理加载器(天空盒)

- DataTextureLoader

hdr 等二进制文件加载器

下面几个loader好像都和hdr有关

```
- EXRLoader
- LogLuvLoader
- RGBELoader： hdr文件
- RGBMLoader
- TGALoader
```

- CompressedTextureLoader

基于块的纹理加载器

```
dds： 微软Direct3d纹理
pvr
```

基于块的纹理加载器 (dds, pvr, ...)的抽象类。

## 材质

## sprite

## 光

DirectionalLight: 直射光

AmbientLight: 环境光

HemisphereLight: 半球光

## CSS2DRender

CSS2DObject

## 动画

模型文件 animations 属性存储了模型的动画，我们需要按照 Three 的模式来启动动画帧。

- AnimationMixer: 混合器

Three 场景中的动画对象对需要使用混合器，混合器控制模型的移动，因此后续通过更新混合器来实现动画帧的切换效果。

- AnimationAction: 动画的控制模块。

动画效果通常由多个动作组成，该组件负责将每个动作进行动画剪辑，每个动画剪辑对应一个动作，该组件来控制动作的开启与暂停等控制功能。

- tick 方法: 动画剪辑更新

AnimationAction 只是启动了动画，但动画真正开始播放需要更新渲染循环中的混合器。AnimationMixer 提供了 update 方法，允许根据时间参数来进行更新，通常使用帧——渲染循环更新时进行更新。

```js
// 在 setupModel 中实现上述逻辑
function setupModel(data) {
  const model = data.scene.children[0];
  // 获取到动画
  const clip = data.animations[0];
  // 声明混合器
  const mixer = new AnimationMixer(model);
  // 将动画按照动作进行动画剪辑
  const action = mixer.clipAction(clip);
  action.play();
  // 更新混合器
  model.tick = (delta) => mixer.update(delta);
  return model;
}
```

```js
// 鸟类模型的总数组
birds = [];

// 获取模型后，放置到 birds 中
const { parrot, flamingo, stork } = await loadBirds();
birds.push(parrot, flamingo, stork);

// 修改渲染循环逻辑，每一帧更新混合器
// 帧时间参数通过 Three 提供的 Clock 来获取
const clock = new Clock();
const render = () => {
  renderer.render(scene, camera);
  // 获取时间参数
  const delta = clock.getDelta();
  // 每一帧更新时更新混合器
  birds.forEach((bird) => {
    bird.tick(delta);
  });
  controls && controls.update();
  requestAnimationFrame(render);
};
```

## 例子

星空： <https://codepen.io/nikita_ska/pen/bqNdBj>
