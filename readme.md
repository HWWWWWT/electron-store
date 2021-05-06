# 秀站网编辑器

## 目录
- [背景](#背景)
- [安装](#安装)
- [使用](#使用)
- [技术点](#技术点)
  - [Vue](#Vue)
    - [组件之间传值](#组件之间传值)
    - [非父子组件之间传值](#非父子组件之间传值)
    - [追踪变化](#追踪变化)
    - [检测变化的注意事项](#检测变化的注意事项)
    - [nextTick](#nextTick)
  - [vuex](#vuex)
  - [axios](#axios)
  - [GSAP](#GSAP)
    - [tweenmax](#tweenmax)

## 背景

是一款免费在线制作MG动画产品。用户可添加素材到画布中，为素材添加动画，最终输出动画视频。
## 安装

```
git clone

cd editor

npm install
```
项目根目录下配置.npmrc文件，为某些项目依赖库如 `node-sass`、`electron`配置淘宝镜像，避免所有依赖都从淘宝镜像下载出现依赖不正确的问题。
<br>
<br>

## 使用
下载chrome浏览器，并将其设置成默认启动浏览器。
```
npm run dev or npm start
```
浏览器默认打开localhost:8088/edit/1，更改草稿编号，避免使用其他开发小伙伴的测试草稿。

## 技术点
### Vue
#### 组件之间传值
[通过Prop向子组件传递数据](https://cn.vuejs.org/v2/guide/components.html#%E9%80%9A%E8%BF%87-Prop-%E5%90%91%E5%AD%90%E7%BB%84%E4%BB%B6%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE)<br>
关键词： 单向传递、async事件修饰符、prop传入引用类型时
#### 非父子组件之间传值
依赖vue实例的 `$emit`、`$on`, 实现了发布订阅模式。
注意：组件销毁时，需要调用`$off`进行取消订阅，否则会触发多次订阅事件。
#### [update](https://cn.vuejs.org/v2/guide/reactivity.html#%E5%A6%82%E4%BD%95%E8%BF%BD%E8%B8%AA%E5%8F%98%E5%8C%96)
`vue2+` 使用 `Object.defineProperty` 将vue组件里的`propoty` 转为 `getter/setter`<br>
`vue3+` 使用 `proxy`<br>
`vue2` 中每个组件实例都对应一个 `watcher` 实例，它会在组件渲染的过程中把“接触”过的数据 property 记录为依赖。之后当依赖项的 setter 触发时，会通知 `watcher`，从而使它关联的组件重新渲染。使用`vue2`时，需要编写更小颗粒度的组件，实现性能优化。<br>
`vue3` 则将每个 `dom` 都插入了唯一标识，只对数据改变的`dom`做重新渲染的动作
