## Vue、CSS、import 属性代码风格

### 组件选项顺序
在组件开发时，组件中的代码结构也应该按照一定的顺序排列，这样更有利于阅读代码，比如计算属性computed应该放在data的后面，因为计算属性是依赖data的；同样地，如果有两个计算属性A、B，A是由B计算出来的，那么A应该放在B的后面，谁在前面，直觉上就是先计算谁，所以应该将代码位置顺序和代码执行顺序保持一致。

Vue2的组件官方推荐顺序如下，我们可以作为参考，其他框架也是一样的道理。
1. 副作用 (触发组件外的影响)
   * el
2. 全局感知 (要求组件以外的知识)
   * name
   * parent
3. 组件类型 (更改组件的类型)
   * functional
4. 模板修改器 (改变模板的编译方式)
   * delimiters
   * comments
5. 模板依赖 (模板内使用的资源)
    * components
    * directives
    * filters
6. 组合 (向选项里合并 property)
    * extends
    * mixins
7. 接口 (组件的接口)
   * inheritAttrs
   * model
   * props/propsData
8. 本地状态 (本地的响应式 property)
   * data
   * computed
9. 事件 (通过响应式事件触发的回调)
    * watch
    * 生命周期钩子 (按照它们被调用的顺序)
      * beforeCreate
      * created
      * beforeMount
      * mounted
      * beforeUpdate
      * updated
      * activated
      * deactivated
      * beforeDestroy
      * destroyed
10. 非响应式的 property (不依赖响应系统的实例 property)
    * methods
11. 渲染 (组件输出的声明式描述)
    * template/render
    * renderError

### 元素 attribute 的顺序
合理的Dom元素顺序也能够提升可读性，可以把重要的属性放在前面，如v-if条件、v-model等，这样可以在第一时间找到你想要的内容；不同类型的属性顺序也应该遵循一定的规范，比如我们通常把事件放在最后配置，这样当你想要查找组件的@click事件时，只要查看最后几条属性就行。

我们以Vue推荐的元素attribute 顺序为例，来体会下属性顺序应该怎么安排。
1. 定义 (提供组件的选项)
   * is
2. 列表渲染 (创建多个变化的相同元素)
   * v-for
3. 条件渲染 (元素是否渲染/显示)
   * v-if
   * v-else-if
   * v-else
   * v-show
   * v-cloak
4. 渲染方式 (改变元素的渲染方式)
   * v-pre
   * v-once
5. 全局感知 (需要超越组件的知识)
   * id
6. 唯一的 attribute (需要唯一值的 attribute)
   * ref
   * key
7. 双向绑定 (把绑定和事件结合起来)
   * v-model
8. 其它 attribute (所有普通的绑定或未绑定的 attribute)
9. 事件 (组件事件监听器)
10. 内容 (覆写元素的内容)
    * v-html
    * v-text

### CSS属性顺序
虽然CSS规范并没有规定特定的属性顺序，但在编写CSS时，保持一致的属性顺序可以使代码更加易于阅读和维护。

比如下面这个，你觉得好理解.box的样式吗？
```css
.box {
    font-size: 14px;
    border: 1px solid #ccc;
    width: 100px;
    left: 10px;
    color: #fff;
    position: absolute;
    background: red;
}
```
编写CSS的顺序也应该和我们的直觉顺序保持一致，在设置一个元素样式时，我们肯定先要知道它在屏幕的位置，然后要知道它的盒子模型的一些信息，包括大小啊边框啊内边距之类的，接着是盒子内部的背景，最后再是盒子内部的文本样式等，大致遵循一个由外及内的顺序。
一般推荐的顺序：
1. 布局和定位相关
   * display
   * position (包括 top, right, bottom, left, z-index)
   * float
   * clear
   * visibility
   * overflow
2. 盒模型相关
   * box-sizing
   * width、height
   * margin、padding
   * border
3. 背景
   * background-color、background-image、background-repeat、background-position、background-size
4. 文本相关
   * color
   * font
   * text-decoration
   * text-align
   * vertical-align
   * ...
5. 其他
上述只是推荐顺序，并不是绝对正确，大家可以根据自己的习惯来调整，但整体顺序是这样的。

如果按照这个顺序，我们再来修改下上面的.box样式，对比下是不是清晰多了
```css
.box {
    position: absolute;
    left: 10px;
    width: 100px;
    border: 1px solid #ccc;
    background: red;
    color: #fff;
    font-size: 14px;
}
```
另外一些相关性较强的属性应该放置在一起，比如display设为flex时，flex相关的属性应该紧密放置在一起，position设为absolute时，left、top等属性应该紧随其后，这样更方便阅读。

### import顺序
我们经常会看到混乱的import顺序，这块也是大家经常忽略的。

通常可以按照如下顺序来导入：
1. 系统内置方法
如Node.js的内置模块
```javascript
import fs from 'fs';  
import path from 'path';
```
2. 第三方模块
```javascript
import axios from 'axios';  
import _ from 'lodash';
```
3. 组件
组件再按照公共组件、业务组件和当前组件的特有的子组件的顺序来引入。
```javascript
import CommonButton from 'base/CommonButton';  
import UserInfo from 'user/UserInfo';  
import ChildComponent from './ChildComponent';
```
4. api相关service
```javascript
import userService from 'user/service';  
```
5. 配置及utils
```javascript
import USER_STATUS from 'user/status';
import {formatMobile} from 'utils/format';
```

### 统一表达
一个团队沟通要想高效，必须使用相同的语言，一致的表达方式，可以减少不必要的学习及认知成本，写代码也是一样。 特别是在涉及一些业务问题时，同一个业务，如果使用不同的词语来表达，难免增加不必要的沟通理解成本。

#### 1. 制定一个领域词汇表
你的业务中一定有很多特有的概念，可以将业务领域内的这些概念抽取出来形成一个词汇表，一般为名词，代指业务中涉及的资源和概念。 一来后续新人进入可以通过这些名词来了解业务的大概面貌； 二来统一了大家在业务方面的认知，减少沟通成本，看到这个命名的词就知道别人在表达啥业务，而不是同一个业务不同人用不同的词语表达。

比如user、member、customer都可能用来表示用户，那么什么时候用哪个词怎么界定呢？如果没有规范随便命名，可能会导致理解上的偏差，你以为是客户，实际别人要表示的是注册用户，无形中增加了很多理解上的障碍，甚至是误导。
领域词汇表示例：
|  变量名 | 含义  |  备注 |
|  ----   | ----  | ---- |
| USER  | 普通注册用户 |
| CUSTOMER  | 客户 |   签订过合同的用户   |
| SKU    | 最小存货单位 |      |
| COMMODITY | 商品 |
| PRODUCT | 产品 |
| CONTRACT | 合同 |
| ORDER| 订单 |

领域词汇表的作用是用来统一认知，在变量命名时根据领域词汇表中的名称来命名，让代码更加可读。

除了业务领域统一用词外，还有一些通用的字段也可以规范下来，比如创建时间到底是用created_time还是created_at，创建人是created_by还是creator，虽然表达含义并无多大区别，但是如果多个地方随机变化使用，也会让人感觉不够规范专业。

#### 2. 统一命名格式
一个团队必须统一命名格式，不同格式的命名表达不同的含义，通过命名就可以知道变量是做什么用的， 而且也让外人看来，更加的专业，增强信任感。试想一下，假如我们对外提供了一个接口，有的返回的变量格式是下划线， 有的是大驼峰，别人又怎么能相信功能是靠谱的呢？
一般常见的命名规范如下：
* 变量：小驼峰，如newsDetail
* 函数：小驼峰，如getNewsDetail
* 类名：大驼峰，如EventBus
* 常量：大写+下划线分割，如NEWS_STATUS
* 目录：小写+中划线分割，如file-name
* 文件：小写+中划线分割，如 event-bus.js，团队内部统一即可
* 组件：大驼峰，如FormTable.vue，如果是目录下的index文件，则用小写，如index.vue
* css class：css类名一般应为小写+中划线，参考html元素的规范，html中一般小写+中划线，如form-title，而不是formTitle

有了规范，在CR时就可以针对问题进行评论，否则有时候会感觉，这样也行那样也行，最后造成代码的混乱。




