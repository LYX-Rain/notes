# 在vue中引入swiper

- 步骤一：安装vue

```bash
# 最新稳定版本
$ npm install vue
# 最新稳定 CSP 兼容版本
$ npm install vue@csp
```

- 如果你已经安装了vue，可以忽略。

- 步骤二：创建vue项目

```bash
# 全局安装 vue-cli
$ npm install -g vue-cli
# 创建一个基于 "webpack" 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm install
$ npm run dev
```

- 步骤三：下载swiper相关的js，然后导入到static文件夹下或者安装vue-swiper（推荐）

```bash
$ npm install vue-awesome-swiper --save
```

- 步骤四：安装runtime：

```
npm install babel-runtime
```

- 在.vue文件中使用

```html
<script>
import { swiper, swiperSlide } from 'vue-awesome-swiper'
 import Swiper from './../static/swiper.min.js'     //引入swiper.js
 require('./../static/swiper-3.3.1.min.css')

 export default {
   mounted () {
    var mySwiper = new Swiper('.swiper-container', {    //挂载创建swiper实例
        direction: 'horizontal',        //滑动方向
        loop: true,                     //是否可循环（能从最后一个滑块滑到第一个）
        pagination: '.swiper-pagination',
        nextButton: '.swiper-button-next',
        prevButton: '.swiper-button-prev'
    })
   }
 }
</script>
<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../static/swiper.min.css";        //css建议这样引入
</style>
```