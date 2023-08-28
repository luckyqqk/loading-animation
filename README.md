# loading-animation
让css动画不再神秘

## 立意说明
本仓库的的目的在于说明动画是如何实现的。
核心代码仅仅是一个html文件。
为什么选择用基础的html，是因为当下2023年，
前端技术的载体框架，像vue，react等，拥有其特定的语法，且需要启动开发服务器才能看到效果。
对比而言，用html文件传播的优势如下：
1. 用浏览器打开即可查看效果。
2. 足以说明问题。
3. 基础的，才更接近原生，离浏览器加载过程更近。
## 作品展示
一个比较基础的css加载动画，麻雀虽小五脏俱全，做动画所需要的技术层，都涉及到了。

![gif](https://github.com/luckyqqk/loading-animation/blob/main/assets/loading.gif)

## 实现思路
1. 先用css渲染一个正方形，表示该动画区域的大小。
2. 想象出这个正方形的内切圆。
3. 再用js循环创建8个子元素的圆，定义子圆的基础半径和其半径增量。
4. 将8个子圆以每45°平均放在内切圆上。
5. 加入animation动画，让父元素一直转动。
先布局，再转动，布局如下：

![image](https://github.com/luckyqqk/loading-animation/blob/main/assets/loading.png)

## 实现细节
先定义父类：
```
<div class="load-wrapper"></div>
```
js基础数值设定（还得留意查看每行代码注释）：
```
const l = 16;  // 内切圆直径
const r = 8;    // 内切圆半径
const min = 2;  // 子圆基础大小
const xs = 0.4; // 子圆增长系数
const padding = min + 8 * xs;  // 最大子圆直径, 也是父类内边距
```
js中，获取父元素，动态设置父类大小（--size），
再生成8个子圆，定义每个子圆大小以及大小增量。

注意：如果在vue或者小程序中，更推荐使用将子圆数据存在一个数组中，
然后在模版中用v-for循环生成子元素的方式实现。
```
const wrapper = document.querySelector('.load-wrapper');
wrapper.style=`--size:${l + padding}px`;
let newEle = null;
for (let i = 0; i < 8; i++) {
  const size = min + i * xs;
  const x = r + r * Math.cos(i * Math.PI / 4) + (padding - size)/2;  // 用三角函数计算子圆x轴偏移
  const y = r + r * Math.sin(i * Math.PI / 4) + (padding - size)/2;
  newEle = document.createElement('div');
  newEle.className = 'load-item';
  newEle.style = `--x:${x}px;--y:${y}px;--size:${size}px`;
  wrapper.appendChild(newEle);
}
```
下面是对应的css代码：
```
.load-wrapper {
  position: relative;
  width: var(--size);
  height: var(--size);
  animation: turning 3s linear 0s infinite;  // 执行动画：3秒一周，匀速重复执行。
}
.load-item {
  position: absolute;
  left: var(--x);
  top: var(--y);
  width: var(--size);
  height: var(--size);
  background-color: #4E86EB;
  border-radius: 50%;
} 
@keyframes turning {  // 动画帧频组：旋转一周/360°
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
```

## 结语
本仓库用一个html，实现了一个加载动画。
技术层包含js，css，html，以及js与html和css的交互技术。
更包含了css中动画实现的技术。
物尽其用，方现其才。拥有好的想法，才能显现出技术的作用。
所以，请尽情的去思考想实现的动画吧。只要有想法，可随意套用本仓库的实现思路。
