# 前端技术栈介绍
主要内容：

> * 前端是做什么的
> * 主流浏览器 && 区别
> * 前端技术栈 react  react-router   redux   webpack   ant-design


## 前端  是做什么的
前端  是负责将设计师设计的产品，包括页面的具体展示形式，以及具体的产品逻辑，用代码的形式在浏览器上实现出来。比如用户的点击，滑动等等这些在产品使用时的操作都需要前端实现相应的逻辑，将用户的动作传达给后端，以达到数据的落入

[页面操作 示例](https://app.creams.io/project/set/buildingSet)

## 主流浏览器 && 区别

![cffa9315fd586df1b97f57139df63e69.jpeg](evernotecid://0EA9DC88-1E3E-4373-ABE0-3A9B15A376C2/appyinxiangcom/22806249/ENResource/p3)

#### 1.渲染引擎和JavaScript引擎 
JavaScript引擎：够提供执行JavaScript代码的运行环境 
渲染引擎: 浏览器内核
#### 2.浏览器的差异，为什么会存在兼容性：
浏览器内核决定了浏览器如何显示网页的内容以及页面的格式信息，不同的浏览器内核对网页编写语法的解释也有不同，因此同一网页在不同的内核的浏览器里的渲染（显示）效果也可能不同

#### 3.现在常用的主流的浏览器
| 浏览器       | 浏览器内核   |  市场占有率(2018/6)  |
| --------   | -----:  | :----:  |
| Chorme     | Blink |   58.95%     |
| Safari        |   Webkit   |   13.69%   |
| FireFox        |    Gecko    |  5.18%  |
| IE        |    Trident    |  3.14%  |

#### 4.浏览器对比

##### Internet Explorer
微软推出的IE浏览器 只要是微软推出的系统都会默认安装IE浏览器，使大部分用户都习惯使用IE，由于市场份额比较大，曾经出现脱离了W3C标准的时候，同时IE版本比较多，存在很多的兼容性问题，更新慢，所以对新web技术不支持，IE能够安全地进入各个网银进行支付操作
##### Firefox
开放源代码、以C++编写的网页排版引擎，是跨平台的，运行速度很快
##### Chrome
浏览器性能很强，安全性、稳定性、流畅性方面体验很棒，界面非常简洁，用户体验好，支持硬件加速，能安装扩展软件如屏蔽广告等等，够最快兼容最新的web前端技术，chrome支持全部HTML5和CSS3功能，能够展现所有网页特效和高级功能

##### Safari
这是苹果公司开发的内核，也是其旗下产品safari浏览器使用的内核。
Chrome、360极速浏览器以及搜狗高速浏览器基于Webkit内核开发的

## 前端技术栈

### 前端发展方向 && mvc框架

### react框架

![dva](https://img-blog.csdn.net/20180415145834303?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hmeTE1MzUy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 1.virtual dom（虚拟dom ） && diff 算法
具有batching(批处理)和高效的Diff算法
Virtual DOM的优势不在于单次的操作,而是在大量、频繁的数据更新下，能够对视图进行合理、高效的更新。这一点是原生操作远远无法替代的。
> * Virtual DOM 在牺牲部分性能的前提下，增加了可维护性，这也是很多框架的通性
> * 实现了对DOM的集中化操作，在数据改变时先对虚拟DOM进行修改，再反映到
> * 真实的DOM中，用最小的代价来更新DOM，提高效率
> * 打开了函数式UI编程的大门
> * 可以渲染到DOM以外的端，比如ReactNative



##### virtual dom

```
<script>
    function Element(tagName, props, children){
        this.tagName = tagName;
        this.props = props;
        this.children = children;
    }
    Element.prototype.render = function(){
        let el = document.createElement(this.tagName);
            props = this.props;
        for(var propName in props){
            el.setAttribute(propName, props[propName])
        }
        this.children.forEach(child => {
            var childEl = (typeof child !== 'string')
                ?  new  Element(child.tagName, child.props, child.children).render()
                : document.createTextNode(child)
            el.appendChild(childEl)
        })
        return el;
    }
    var ul = new Element('ul',{id:'list'},[
        { tagName: 'li', props: { id: 'item' }, children: [
            { tagName: 'span', props: { id: 'spanele' }, children: ['span 1'] },
            { tagName: 'span', props: { id: 'spanele' }, children: ['span 2'] }
        ] },
        { tagName: 'li', props: { id: 'item' }, children: ['Item 2'] },
        { tagName: 'li', props: { id: 'item' }, children: ['Item 3'] }
    ])
    ulRoot = ul.render();
    document.body.appendChild(ulRoot);
</script>
```

##### diff 算法
```
<script>
    // js书写 element json
    // {
    //     tagName: 'li', props: { id: 'item' }, children: [
    //         { tagName: 'span', props: { id: 'spanele' }, children: ['span 1'] },
    //         { tagName: 'span', props: { id: 'spanele' }, children: ['span 2'] }
    //     ]
    // }

    let result = [];
    const diffLeafs = function (beforeLeaf, afterLeaf) {
        //获取较大节点树的长度
        let count = Math.max(beforeLeaf.children.length, afterLeaf.children.length);
        for (let i = 0; i < count; i++) {
            const beforeTag = beforeLeaf[i];
            const afterTag = afterLeaf[i];
            if (beforeTag === undefined) {
                //添加 afterTag 节点
                result.push({ type: "add", elment: afterTag });
            } else if (afterTag == undefined) {
                //删除 beforeTag 节点
                result.push({ type: "remove", elment: beforeTag });
            } else if (beforeTag.tagName !== afterTag.tagName) {
                //节点名字改变时 删除 beforeTag 添加 afterTag
                result.push({ type: 'remove', elment: beforeTag });
                result.push({ type: 'add', elment: afterTag });
            } else if (beforeTag.innerHtml !== afterTag.innerHtml) {
                //节点不变而内容改变时 改变节点 之前的节点没有内容 新节点替换旧节点
                if (beforeTag.children.length === 0) {
                    result.push({
                        type: 'changed',
                        beforeElment: beforeTag,
                        afterElment: afterTag,
                        html: afterTag.innerHtml
                    });
                }else{
                    diffLeafs(beforeTag, afterTag)
                }
            }
        }
    }

    // result 就是diff算法比较后得到的 等待执行的 更新队列 batch
</script>
```

##### 参考文章
[如何实现一个 Virtual DOM 算法](https://github.com/livoras/blog/issues/13) <br/>
[理解 Virtual DOM](https://github.com/y8n/blog/issues/5)


#### 2.JSX（JS+XML）
##### angular 模版语法

```
<div id="app">
  {{ message }}
</div>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

#### react jsx
```
class HelloWord extends React.Component{
  render() {
    return (
      <p>
        Hello,<input type='text' placeholder='Your name here' />!
        It is {this.props.data.toTineString()}
      </p>
    )
  }
}

setInterval(function () {
  React.render(
    <HelloWord data={new Date()} />
    document.getElementById('example')
  )
})
```

##### 对比

> * 语义化
    提供更加语义化且易懂的标签(将传统的HTML标签封装成React组件)，我们就可以像使用HTML标签一样使用这个组件
> * 直观
    JSX 让小组件更加简单、明了、直观。在有上百个组件及更深层标签树的大项目中，这种好处会成倍地放大。在函数作用域内，使用 jsx 语法的版本与使用原生 JavaScript 相比，其标签的意图变得更加直观，可读性也更高
> * 模版语法 对比
    JSX 以干净切简洁的方式保证了组件中的标签与所有业务逻辑的相互分离。它不仅提供了一个清晰、直观的方式来描述组件树，同时还让你的应用程序更加符合逻辑

#### Component 组件化 模块化 开发
React是面向组件编程的(组件化编码开发)，最后得到标签代码 
React 的一切都是基于组件的。可以通过定义一个组件，然后在其他的组件中，可以像HTML标签一样引用它。说得通俗点，组件其实就是自定义的标签。
利于项目重构与移植

```
function MyComponent() {
  return <h1>自定义无状态组建(最简洁，推荐使用，没有生命周期，性能最优)</h1>
}

class MyComponent extends React.Component{
  render() {
    return <h1>ES6类语法(复杂组建，推荐使用) react声明式编码</h1>
  }
}

var MyComponent = React.createClass({
  render() {
    return  <h1>ES5 老语法 (不推荐使用)</h1>
  }
})

// 创建完成 渲染页面
ReactDOM.render(<MyComponent />, document.getElementById('example'));
```

#### 生态圈

> * facebook开源前端框架，由facebook团队研发，有强大的生态圈和用户量，很适合构建大型项目
> * 实现多端复用
Facebook内部Ads Manager iOS版本由7位前端工程师用React Native花了5个月完成。而Android版本，是同一班人，3个月内完成。代码重用率达到了87%


### react-router 单页面应用
> * 分离前后端关注点，前端负责view，后端负责model，各司其职,可以缓存较多数据，减少服务器压力
> * 单页应用可以拥有和桌面应用一样的响应速度—尽可能地把（临时的）工作数据和处理过程从服务端转移到浏览器端，单页应用由此把响应时间减至最小，拥有更好的用户体验


### react-redux 数据管理
![b3aaaccb9f03d03dc921c07c59b9b009.jpeg](evernotecid://0EA9DC88-1E3E-4373-ABE0-3A9B15A376C2/appyinxiangcom/22806249/ENResource/p2)

首先 redux 可以认为是 发布/订阅 模型的一种实现，组件通过 action 发布事件，经过 reducer 处理后，将改变后的数据通知到订阅 stroe 的组件，然后触发监听 store 的组件的变动，像后端的 mq， kafka

##### Redux 的设计思想很简单，就两句话
> * Web应用是一个状态机，试图与状态是一一对应的。
> * 所有的状态，保存在一个对象里面redux 通过 store 存储几乎所有的数据信息，将数据平铺在一个大 JSON 中，便于各处获取使用。

![](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg)



[参考文章__ 阮一峰 redux 入门](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

### webpack 前端工程化开发
webpack 是一个现代 JavaScript 应用程序的模块打包器(module bundler)，将有依赖关系的模块打包成静态资源

> * production
    splitChunks将依赖树拆分,整合第三方库vendor和公共模块common
    HappyPack让 Webpack 同一时刻处理多个任务，发挥多核 CPU 电脑的威力，以提升构建速度
    es6的import异步加载，可以进行按需加载，优化加载速度，保证首屏加载速度
> * development
    hmr(hot-module-replace)增加开发体验和速度
    第三方loader less-loader，file-loader等 编译文件
    第三方plugin 扩展webpack


### ant-design
   [阿里旗下蚂蚁金服开源组件库](https://ant.design/docs/react/introduce-cn
)
