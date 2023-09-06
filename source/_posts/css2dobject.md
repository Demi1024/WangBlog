---
title: Three.JS创建中文的几种方式
date: 2020-06-16 21:59:18
tags: 
  - CSS2DObject
  - TextLoader
  - Sprite
categories:
  - Three.js
---

**导语：** 在进行3D开发时，常常需要给物体加些中文文字进行提示。由于Three.JS不自带对中文字体的支持，在开发中碰壁无数。在经历了多次的探索和实践后，总结了几种常用到的中文字体加载方式，并对比优缺点。

首先，先给出官网的创建文字的地址，有需要的同学可以前去学习。[Three.JS官网](https://threejs.org/docs/index.html#manual/zh/introduction/Creating-text)

## （一）加载字体纹理

此方法采用的是，先将需要添加的文字在画布（Canvas）中绘制出来，然后转换成纹理（Texture），最后应用于精灵（Sprite）中。

效果如图所示

![通过纹理方式加载文字](http://ww1.sinaimg.cn/mw690/0063Qlsmgy1ggbou6uga8j30jj0h0jtj.jpg)

话不多说直接上代码

```javascript

// 生成添加文字的贴面材质
function makeTextTexture(message, color,backgroundColor = { r:255, g:255, b:255, a:1.0 }) {
  /* 创建画布 */
  let canvas = document.createElement('canvas');
  let context = canvas.getContext('2d');
  /* 设置画布大小 */
  canvas.width = 650;
  canvas.height = 300;
  /* 背景颜色 */
  context.fillStyle   = "rgba(" + backgroundColor.r + "," + backgroundColor.g + ","
      + backgroundColor.b + "," + backgroundColor.a + ")";
  /* 支持多行文字 */
  let chr = message.split("\n");
  let temp = "";
  let row = [];

  context.font = "56px Arial";
  // 水平对齐
  context.textAlign = 'left ';
  // 垂直对齐
  context.textBaseline = "middle";
  /* 字体颜色 */
  context.fillStyle = color;
  for (let a = 0; a < chr.length; a++) {
    row.push(temp);
    temp = "";
    temp += chr[a];
  }

  row.push(temp);

  for (let b = 0; b < row.length; b++) {
    let fontNum = row[b].length
    context.fillText(row[b], 30, 0 + b * 80, fontNum * 80);
  }
  /* 画布内容用于纹理贴图 */
  let texture = new THREE.Texture(canvas);
  texture.needsUpdate = true;

  return texture
}

function makeTextSprite(textName, message, color, textPosX, textPosY, textPosZ) {
  let texture = makeTextTexture(message, color)
  let spriteMaterial = new THREE.SpriteMaterial({
    map: texture,
    transparent: true,
    opacity: 0.9,
    depthTest: false,
    fog: false
  });
  let sprite = new THREE.Sprite(spriteMaterial);
  /* 缩放比例 */
  sprite.scale.set(60, 30, 60);
  sprite.position.set(textPosX, textPosY, textPosZ);
  sprite.name = textName;
  // 向场景中添加文字
  scene.add(sprite)；
}
```

## （二）Three.JS自带的文字几何体

使用Three.JS的TextGeometry，创建文字几何体。

此方法需要预先加载字体库，之后在创建文字几何体时应用字体库。我这里加载了微软雅黑的字体文件（json格式）

效果如图所示

![加载字体方式](http://ww1.sinaimg.cn/mw690/0063Qlsmgy1ggbowh29p1j30oe0evwgd.jpg)

代码如下

```javascript
let fontRes = null;
// 加载字体包Font
function loadFont(){
  let fontLoader = new THREE.FontLoader();
  if(process.env.NODE_ENV === 'development'){
    fontLoader.load('/assets/Font/YaHei_Regular.json', function(response) {
      fontRes = response;
    });
}
// 加载3D文字
function loadText(textName, content, posX, posY, posZ,fontCol=0xFFFFFF) {
  let cMesh = null
  // 创建3D 文字
  let textGeometry = new THREE.TextGeometry( content , {
    font: fontRes,
    size: 15,
    height: 1,
    curveSegments: 4,
    bevelEnabled: false,
  } );
  textGeometry.computeBoundingBox();
  //创建法向量材质
  let textMaterial = new THREE.MeshBasicMaterial({
    color: fontCol,
    transparent: true,
    opacity: 0.8
  });
  cMesh = new THREE.Mesh(textGeometry, textMaterial);
  cMesh.position.set(posX, posY, posZ);
  cMesh.name = textName;
  scene.add(cMesh);

}

```

通常一个完整的字体json文件，体积都比较大。如果只用到极少的字体，去加载一个完整的字体文件是不划算的。推荐通过精简字体文件，提取包含使用文字的文件，通过减小字体文件的体积来优化加载速度

## （三）CSS2Dobject 加载文字

我们都知道通过css控制dom元素是非常方便的，那么怎么在Three.js中也实现这种加载方式呢？

通过查阅Three.js项目示例我找到了灵感。[例子参阅](https://github.com/mrdoob/three.js/tree/dev/examples) 在`css2d_label.html`这个文件中我看到文字吸附于模型的示例

原理是创建一个

下面来看段代码

```javascript
alert('Hello World!');
```
