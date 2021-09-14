## Vue相关API

### 1)全局配置

-  vue.config 对象的属性

- Vue.config.productionTip = false 控制浏览器的控制台中的提示信息是否显示

### 2)全局API

- vue.component():注册公共组件或者说注册全局组件的,main.js中可以在某个js文件中书写,然后在main.js中引入components注册的是局部组件

- vue.directive(): 设置指令的

- Vue.filter():过滤器(设置曰期时间的格式化)

- Vue.use():声明并使用插件

- vue.nextTick(callback)y:

  - 属性值的编辑和查石模式,鼠标点击内容,进入到编辑模式,鼠标离开进入到查看模式,点击文字内容,进入到编辑模式后,自动获取光标.
  - 你的项目中遇到过什么样的问题,如何解决的?
  - swiper组件,该组件对象必须要等到页面中的DOM加载完毕后,才可以创建swiper对象,然后轮播图才有效果
  - 创建swiper对象的代码放在了mounted生命周期中(页面加载完毕了),但是,轮播图的数据是异步诘求回来的,此时有可能会出现这样的问题:页面已经加载完毕了,swiper对象也创建完毕了,但是轮播图的数据还没有获取到,此时页面中的轮指图效果有可能会失效,那就只能等到轮播图的数据请求完毕后(页面也渲染完了),才能创建swiper对象,才有意义
  - 通过watch监视的方式来解决该问题,但是也不行,默认监视中的代码是不执行的,只有监视的数据发生了变化,watch才有意义,而且不确定到底是什么时候获取到的响应的数据,不能准确的把控响应数据回来的时间,再创建swiper对象
  - 使用watch+定时器创建swiper对象
  - 最好的方式:watch+nextTick()中的回调来创建swiper对象
  - 也有不合适的地方,当前的Vue.nextTick()方法,先要引入Vue,组件中如果使用nextTick方法,完全可以使用this.$nextTick()来使用

- Vue.set()

  ```
  
  在Vue的组件中, data中的数据是响应式数据,计算属性中的数据也是响应式的
  例子:
  data () {
  	return {
  		product:{ 	//商品对象
  		price: 100l	//价格
  		}
  	}
  }
  此时product对象是响应式的数据对象
  响应式数据:数据发生了变化,页面会随之进行渲染,此时这样的数据叫响应式的数据
  点击按钮,为商品对象添加了一个co1or的属性，
  this.product.color = '红色’此时的这个co1or是不是响应式的数据,页面是不会渲染的
  希望添加的co1or是响应式属性数据
  vue.set( product对象, 'color属性', 值 ),组件中使用vue.set还需要引入Vue,麻烦,所以, this. $set()方法
  
  一定要通过set方法来添加响应式的数据吗?不一定
  还有没有其他的方法设置响应式的数据?有,什么方式?
  mounted方法中，发送异步请求
  async mounted () {
  	// trademarks是一个常量
  	const result = await this.$API.getTrademark() //获取品牌列表信息数据的
  	if(resu1t.code===200){
  		this.trademarks = resu1t.data // 把发送请求获取到的品牌信息列表数据直接保存到了trademarks数组中
  		traemarks.forEach((item)=>{
              // item就是数组中的每个品牌对象,原本item品牌对象中没有color属性
              item.color='red' //此时添加了color属性，间题:co1or属性是不是响应式的?是的
  		})
  		// 下面的这行代码才是保存品牌列表数据
  		this.trademarks = trademarks
  	}
  }
  
  总结:vue中添加响应式的数据的方式有两种: set方法,或者在为data对象中的数据赋值之前添加的数据
  
  ```

- Vue.delete():删除对象的 property。如果对象是响应式的，确保删除能触发更新视图。这个方法主要用于避开 Vue 不能检测到 property 被删除的限制，但是你应该很少会使用它。

- Vue.mixin()方法

  ```
  child.vue组件文件中
  import mixin from './test.js'
  export default {
  	mixins: [mixin]
  }
  student.vue组件文件中
  import mixin from './test.js'
  export default {
  	mixins: [mixin]
  }
  test.js文件件
  export default {
  	data () {
  		return {
  			product: {} //商品对象
  		}
  	}，
  	methods:{
  	
  	}
  }
  总结: 多个组件文件中有相同的选项,那么此时可以把这些.相同的配置选项放在一个单独的js文件中,然后在各个组件的文件中引入进来,通过mixins选项进行配置,这些组件中就都有了js文件中的选项配置
  ```

### 3)配置选项

- 数据

  - data:组件中使用的data函数的形式目的是为了实现数据的响应式,组件中使用data对象的写法是无法实现真正的数据响应式的

  - props:接收父级组件传递进来的数据(属性或者方法),实现父子组件之间的通信

  - computed:计算属性,在组件中某个数据发生变化,相关联的数据会自动的变化,也是响应式的数据

  - methods:这里面是可以实现计算属性的操作的,但是也有区别,计算属性由缓存,methods是没有缓存的

  - watch:监视(侦听)

    - computed和watch的区别:

    ```
    计算属性:组件加载的时候,计算属性会自动的执行，
    当你想要使用某个数据,但是data中没有,且可以根据已有数据计算出来,那么就可以使用计算属性计算出来
    监视:组件加载的时候,默认是不执行,除非数据发生了变化,才会执行,如果监视默认执行需要进行配置(immediate:true),数据发生变化,需要做什么事情,深度监视(deep:true)
    'a.b.c':function(){}监视某一层数据的变化
    ```

- DOM

  - el:选择器
  - template:模板
  - render:渲染虚拟DOM，变成真实的DOM

  动态组件：const view =通过import(路径')此时引入进来的组件，需要使用component组件标签配合is属性来实现页面中的tab栏效果可以使用动态组件的方式来实现

  缓存组件：通过keep-alive标签把某个组件进行包裹,那么此时的这个组件就被缓存起来,频繁切换组件,又不想让切换后的组件重新的创建,那么此时可以进行缓存,tab栏的效果其实就可以使用缓存组件的方式

  异步组件：在需要该组件的时候,组件并不是静态引入,而是动态引入的然后再加载这个组件(页面展现出来后,没有立刻使用某个组件,而是在恰当的时机,才需要展示某个组件内容,该组件可以设置为异步组件),按需引入,优化操作

  函数组件：无状态,无实例,无生命周期,渲染性能更高,轻量级的组件,所需数据通过父级组件进行传递

  递归组件：组件内部调用自己,实现数据的递日(展示)，遍历菜单数据的时候,可以使用递归组件

  路由组件：通过路由进行注册的组件

  ​	前台路由：一种映射关系,地址和组件的关系

  ​	路由链接：地址,router-link标签

  ​	路由视图：显示路由组件,router-view标签

  公共组件︰该组件需要通过Vue.component()进行注册,任意的一个组件中都可以直接使用该组件

  组件：具有特定功能效果的集合,html+css+js

  

  面试题：
  谈谈你对组件的理解:聊各种组件,组件中的选项,什么时候用什么组件,组件的生命周期

  谈谈你对Vue的理解:数据劫持,指令,响应式数据,组件的通信,及上面的内容,路由

  

- 生命周期
  - beforeCreate
  - created
  - beforeMount
  - mounted
  - beforeUpdate
  - updated
  - beforeDestroy
  - destroyed
  - activated 激活
  - deactivated 失活
  - errorCaptured
- 资源
  - directives:可以同时注册多个指令
  - filters:可以同时定义多个过滤器
  - components:可以同时注册多个组件
- 杂项
  - mixins：混合
  - provide / inject：注入组件通信
- 其它
  - name:路由组件,递归组件中必须有name属性
  - functional:设置当前的组件是不是函数组件

### 4)实例属性

- $el
- $parent 父级组件
- $children 当前组件中的直接的所有的子级组件
- $refs 可以获取某个标签对象
- $attrs 获取的父级组件传递过来的所有的属性,不包括props接收的,class和style
- $listeners 获取父级组件传递过来的事件,native修饰是不要的

### 5)实例方法

- 数据
  - $watch()
  - $nextTick()
  - $set()
  - $delete()
- 事件
  - $on() 绑定事件
  - $once() 绑定一次事件
  - $off() 解绑事件
  - $emit() 分发事件
- 生命周期
  - $mount() 挂载
  - $nextTick()
  - $destroy() 销毁

### 6)指令：列出Vue中常用的7个指令

- o v-text 标签中间显示文本,相当于DOM中的innerText
- v-html 标签中间显示标签内容,相当于DOM中的innerHTML
- v-show 设置标签内容显示或者隐藏,操作的是css中的display
- v-if 设置标签显示或者隐藏,在DOM树中是否存在标签
- v-else
- v-else-if
- v-for 遍历数据
- v-on 绑定事件
- v-once
- v-bind 强制数据绑定
- v-model 双向数据绑定
- v-slot 插槽(普通插槽,命名插槽(具名插槽),作用域插槽)

### 7)特殊属性

- key 唯一的标识,diff算法
- ref 获取dom标签对象
- is 设置动态组件的

### 8)内置组件

- component
- transition 过渡效果
- keep-alive 缓存组件
- slot 插槽

## Vue组件的生命周期

### 1)单个组件生命周期

- 初始化：
  - beforeCreate
  - created
  - beforeMount
  - mounted
- 更新：
  - beforeUpdate
  - updated
- 死亡：
  - beforeDestroy
  - destroyed

### 2)父子组件的生命周期

- 初始化:
  - beforeCreate
  - created
  - beforeMount
  - --child beforeCreate
  - --child created
  - --child beforeMount
  - --child mounted
  - mounted
- 更新：
  - beforeUpdate
  - --child beforeUpdate
  - --child updated
  - updated
- 死亡：
  - beforeDestroy
  - -- child beforeDestroy
  - -- child destroyed
  - destroyed

### 3)带缓存的路由组件生命周期

- 初始化:
  - ...
  - mounted
  - --Child activated
  - activated
- 路由离开
  - --Child deactivated
  - deactivated
- 路由回来
  - --Child activated
  - activated

### 4)捕获子组件错误的勾子

- 子组件执行抛出错误
  - errorCaptured

### 5)各个生命周期勾子说明

​		1.只说生命周期的个数,及每个生命周期的作用

​		2.上面的内容+每个生命周期的扩展+源码的分析

​		3.不仅可以描述出来+画图

​			1.子孙组件报错,的生命周期回调

​			2.组件缓存的时候调用的生命周期回调

​			3.描述其他的8个生命周期

![未标题-3](.\media\1.png)

(1) beforeCreate():在实例初始化之后调用, data和methods都还没有初始化完成,通过this不能访问初始化data与methodslcomputed

(2) created():此时data和methods都已初始化完成,可以通过this去操作,可以在此发ajax请求编译模板

(3) beforeMount():模板已经在内存中编译,但还没有挂载到页面上,不能通过ref找到对应的标签对象插入到界面上显示

(4) mounted():页面已经初始显示,可以通过ref找到对应的标签,也可以选择此时发ajax请求



n次更新数据

(5) beforeUpdate():在数据更新之后,界面更新前调用,只能访问到原有的界面更新界面

(6) updated():在界面更新之后调用, 此时可以访问最新的界面

销毁组件/ v-if隐藏/离开不缓存的路由组件
(7) beforeDestroy():实例销毁之前调用, 此时实例仍然可以正常工作

(8) destroyed(): vue 实例销毁后调用,实例已经无法正常工作了

(9) deactivated(): 组件失活,但没有死亡

(10) activated(): 组件激活,被复用

(11) errorCaptured(): 用于捕获子组件的错误,return false可以阻止错误向上冒泡(传递)

```
先说生命周期多少个,每个作用是什么
然后再说生命周期的执行过程(父子组件的生命周期结合)
叙述整个生命周期的流程(源码)
new vue()开始创建实例对象开始进行初始化的操作
beforecreate开始执行,此时执行的时候data和methods都没有初始化
进入到created中,那么此时data和methods已经初始化完毕了(数据代理已经结束---object.defineProperty()原理),数据劫持已经结束(object.defineProperty ,把data中的数据也遍历了,添加到劫持对象的data上,还创建了dep对象, data中有一个属性就会对应着一个dep对象)
进入到beforeMount之前开始进行模板解析（创建虚拟DOM对象,吧e1中对应htm1的根节点放在虚拟DOM对象容器中,遍历所有的子节点,判断节点是标签还是文本,如果是文本,再看是不是插值,标签中的文本替换,如果是标签,获取标签中的所有的属性,判断属性是不是vue的指令,如果是指令,判断这个指令是不是普通指令还是事件指令,最终都会进入到bind方法中,创建watcher监视对象,内部会获取htm1标签中的表达式值,此时会建立dep对象和watcher对象的关系,)
1对1
1对多
多对1
多对多
data中一个属性对应一个dep
htm1的模板中,一个表达式对应一个watcher对象
最终会调用update对象中的方法进行解析最后的替换操作
mounted结束,页面就渲染完毕了
```

## 组件深入

### 1)动态组件

- 通过<component>动态确定要显示的组件，is指定要显示组件的组件名

  ```
  <component :is="currentcomp"/>
  ```

- 问题:当从A组件切换到B组件时,A组件就会销毁

### 2)缓存组件

- 使用<keep-a7ive>缓存动态组件,可以通过include或exclude指定只缓存特定组件

  ```
  <keep-alive :exc1ude="[ 'Home ' ]">
  	<component :is="currentcomp" / >
  </keep-alive>
  ```

  

- 使用<keep-alive>也可以缓存路由组件

  ```
  <keep-alive include="Life1">
  	<router-view></router-view>
  </keep-alive>
  ```

- 路由组件对象什么时候创建?

  - 默认:每次跳转/访问对应的路由路径时
  - 有缓存:第一次访问时

- 路由组件对象什么时候死亡?

  - 默认:离开时
  - 有缓存:离开时不死亡,只有当destroy/父组件死亡/刷新页面

### 3)异步组件

- 好处:能更快的看到当前需要展现的组件界面(异步组件的代码开始没有加载)
- 无论是路由组件还是非路由组件都可以实现异步组件效果
  - 拆分单独打包
  - 需要时才请求加载组件对应的打包文件
- 配置组件: component: () => import(modulePath)
  - import(modulePath):被引入的模块会被单独打包(code split)    --ES8的新语法
  - () =>import(modulePath):函数开始不调用,当第一次需要显示对应的界面时才会执行,请求加载打包文件
- 细节
  - import()返回promise, promise成功的结果就是整体模块对象
  - 本质上:可以利用import()实现对任意模块的懒加载

### 4)函数式组件: functional + render + JsX

- 函数组件的特点

  - 无状态。无法实例化
  - 内部没有任何生命周期处理函数
  - 轻量,渲染性能高,适合只依赖于外部数据传递而变化的组件(展示组件，无逻辑和状态修改)
  - 可以没有根标签

- 编码

  ```
  export default {
  	functiona1 : true, //当前是函数组件
  	render (createElement，context) {
  		return 要显示界面的虚拟DOM
  	}
  }
  ```

### 5)递归组件

- 递归组件:组件内部有自己的子组件标签
- 应用场景:用于显示树状态结构的界面
- 注意:递归组件必须有name
- 编码:实现一个简单的可开关的树状结构界面的Tree 组件

## vue组件间多种通信方式

### 1)组件间通信方式列表

```
1) props----父子组件/子父组件通信
2) vue自定义事件----父子组件
3) 全局事件总线---任意组件
4) v-mode1----父子组件， 本质：input事件+value属性
5) .sync ----父子
6) $attrs与$1isteners----父子
7) $refs，$children与$parent---父子---相对 来说 少一些
4) provide与inject---父和子孙----封装高阶组件的时候使用---相对来讲稍微少一些
9) vuex---任意组件
10) 插槽==>作用域插槽----父子
11) pubsub----不属于vue,--->转到react中 ----任意组件


思路:vue---渐进式框架---组件(各种组件),响应式数据,生命周期，组件通信, react,原型,闭包,预解析,作用域....es6为什么要有组件通信,组件的关系
组件通信
应用例子
自动登录.token
```

### 2)通信方式的选择

```
父子
	props:
		父向子
		子向父
	vue自定义事件:
		子向父
	v-mode1:
		父子之间
	.sync:
		在父向子的基础上添加子向父
	$ref,Schi1dren与Sparent:
		$ref/$chi1dren:父向子
		Sparent:子向父
	插槽==>作用域插槽
		默认插糟/具名插槽:父组件向子组件传递标签内容
		作用域插件:子向父传递数据后，父组件根据接收到的数据来决定向子组件传递不同的标签内容
祖孙
	$sattrs与$1isteners
	provide与inject
兄弟或其它/任意
	全局事件总线
	vuex
```

### 3)各种组件间通信详细说明

#### 1.方式一:props

```
1)．实现父向子通信:属性值是非函数
2)．实现子向父通信:属性值是函数
应用:最基本，用得最多的方式
```

#### 2.方式二: vue自定义事件

```
1).用来实现子组件向父组件通信
2).相关诰法:
	父组件中绑定自定义事件监听:
		<child @eventName="callback">
	子组件中分发事低
		this. $emit('eventName', data)
应用:elment-ui的组件的事件监听语法都用的是自定义事件
	我们项目中的组件也用了不少自定义事件
```

#### 3.方式三:全局事件总线===>消息订阅与发布

```
1)．实现任意组件间通信
2)．编码:
	将入门js中的vm作为全局事件总线对象︰
		beforecreate(){
			vue.prototype.$bus = this
		}
	分发事件/传递数据的组件: this.$bus.$semit('eventName',data)
	处理事件/接收数据的组件: this.$bus.$on('eventName',(data) =>{})
应用:前台项自中使用全局事件总线
```

![image-20210913231248204](.\media\2.png)

#### 4.方式四: v-model

```
1)．实现父子之间相互通信/同步
2)．组件标签上的v-mode1的本质:动态value属性与自定义inpu t监听来接收子组件分发的数据更新父组件数据
	父组件:
		<customInput v-mode1="name " />
		<! --等价于-->
		<customInput :value="name" @input="name=$event" />
	子组件:
		<input type="text" :va1ue="value" @input="$emit('input'，$event.target.value)">
		props: ['value ']
应用: e1ement-ui中的表单项相关组件都用了v-mode1: Input / select / checkbox / Radio
```

#### 5.方式五: .sync

```
1)．实现父子之间相互通信/同步(在原本父向子的基础上增加子向父)
2)．组件标签的属性上使用.sync的本质:通过事件监听来接收子组件分发过来的数据并更新父组件的数据
	父组件:
		<child : money.sync="tota7" />
		<!--等价于-->
		<chi1d : money="tota7" aupdate : money="tota1=$event"/>
		
		data(){
			return {
			total: 1000
			}
		}，
	子组件:
		<button @click="$emit( 'update : money ', money-100)">花钱</button>
		props: ['money']
应用:
element-ui在有显示隐藏的组件上: Dialog / Drawer
```

#### 6.$attrs与$listeners

```
1).$attrs
	实现当前组件的父组件向当前组件的子组件通信
	它是包含所有父组件传入的标签属性(排除props声明，c1ass与sty1e的属性)的对象
	使用:通过v-bind="$attrs"将父组件传入的n个属性数据传递给当前组件的子组件
2).$listeners
	实现当前组件的子组件向当前组件的父组件通信
	$listeners是包含所有父组件传入的自定义事件监听名与对应回调函数的对象
	使用:通过v-on="$listeners”将父组件绑定给当前组件的事件监听绑定给当前组件的子组件
应用:利用它封装了一个自定义的带hover文本提示的el-button
```

#### 7.$refs & $children & $parent

```
1).$refs
	实现父组件向指定子组件通信
	$refs是包含所有有ref属性的标签对象或组件对象的容器对象
	使用:通过 this.$refs.child 得到子组件对象，从而可以直接更新其数据或调用其方法更新数据
2).$chi1dren
	实现父组件向多个子组件通信
	$children是所有直接子组件对象的数组
	使用:通过this.$children遍历子组件对象，从而可以更新多个子组件的数据
3). $parent
	实现子组件向父组件通信
	$parent是当前组件的父组件对象
	使用:通过this.$parent得到父组件对象，从而可以更新父组件的数据
应用:在后台管理项目中使用了$refs
```

#### 8.provide与inject

```
1)．实现祖孙组件间直接通信
2)．使用
	在祖组件中通过provide配置向后代组件提供数据
	在后代组件中通过inject配置来声明接收数据
3)．注意:
	不太建议在应用斤发中使用,一般用来封装vue插件
	provide提供的数据本身不是响应式的==>父组件更新了数据，后代组件不会变化
	provide是俱的数据对象内部是响应式的 ==>父组件更新了数据，后代组件也会变化
应用:element-ui中的Form组件中使用了provide和inject
```

#### 9.Vuex

```
1).实现任意组件问通信
2).vuex是一个专为Vue应用程序设计的管理多组件共享状态数据的 Vue插件
	任意组件都可以读取到Vuex中store的state对象中的数据
	任意组件都可以通过dispatch()或commit()来触发store去更新state中的数据
	一旦state中的数据发生变化，依赖于这些数据的组件就会自动更新
应用:前台和后台项目都有用vuex管理组件数据
```

#### 10.插槽==>作用域插槽slot-scope

```
1)．实现父组件向子组件传递标签内容
2)．什么情况下使用作用域插槽?
	父组件需要向子组件传递标签结构内容
	但决定父组件传递怎样标签结构的数据在子组件中
3)．编码:
	子组件:
		<slot :row="item" :$index="index">	<!-- slot的属性会自动传递给父组件-->
		</s1ot>
	父组件:
		<template s1ot-scope="{row，$index]">
			<span>{f$index+1}]}</span> &nbsp ; &nbsp;
			<span :style="{color: $index%2===1 ? 'blue' : 'green'}" >{{row.text]}</span>
		</ temp1ate>
应用:element-ui中的Table组件
```

## Vue的响应式

### 1)几个重要问题?

- mvvm的理解，与MVC的区别?

  

- 组件的data为什么只能是函数不能是对象?

  - 同一个组件的多个组件实例的data必须是不同的对象(内容初始数据可以相同)
  - 如果是data是对象,组件的多个实例共用一个data对象
  - 如果是函数,组件对象通过调用函数得到的一个新的data对象

- 响应式数据与非响应式数据?

  - 响应式: data / props / computed/ vuex的state与getters
  - 非响应式:仅仅存在于组件对象上的属性数据
    - 给组件对象添加一个新属性: this.xxx = value
    - 直接给一个响应式对象添加一个新属性: this.product.xxx = 'abc' ==> this.$set(this.product, 'xxx'，, 3)

- 对象的响应式与数组的响应式有什么区别?

  - 对象:通过Object.defineProperty()添加setter方法来监视属性数据的改变＋订阅-发布
  - 数组:重新数据一系列的更新数组元素的方法＋订阅-发布

### 2)Vue数据绑定/响应式原理图

![image-20210913235323603](C:\Users\LEGION\Desktop\笔记\vue\media\3.png)



- 理解:
  - 说vue的数据绑定的原理,或者数据响应式的原理,都是在说一个事
  - 当我们修改了data中的数据时,组件界面是如何自动更新的
  - 这里涉及下面几个重点
    - 数据代理: Object.defineProperty() this.msg = 'abc' ==> data.msg = 'abc'
    - 数据劫持/监视: Object.defineProperty()
    - 发布-订阅: observer 与dep 与watcher
    - this._data.msg = 'xxx'
- 数据代理
  - 通过Object.defineProperty()给vm添加与data对象中对应的属性
  - 在getter中,读取data中对应的属性值返回==>当我们通过this.xxx读值时,读取的是data中对应的属性值
  - 在setter中,将最新的值保存到data中对应的属性上==>当我们通过this.xxx = value时, value保存在data中对应的属性上
  - 作用:简化对vm/组件对象内部的data对象的属性数据的操作(读/写)
- 数据劫持/监视
  - 在observer中,通过Object.defineProperty()给data中所有层次属性都添加上getterlsetter
  - 为每个属性都创建一个dep对象,用于后面更新
  - 注意:在解析模板时,为每个表达式都创建了一个用于更新对应节点的watcher
  - 在getter中,去建立dep与watcher之间的关系
    - dep与data中的属性一一对应
    - watcher与模板中的表达式——对应
    - 一个dep中,保存了包含n个watcher的数组==>当多个表达式用到当前dep所对应的属性
    - 一个watcher中,保存了包含n个dep的对象==→>当表达式是一个我层的表达式
  - 在setter中,通过dep去通知所有watcher去更新对应的节点

- 发布-订阅模式
  - 发布者: observer
  - 订阅者: watcher
  - 订阅器/中间人: dep
- 初始化
  - 实现数据代理:通过defineproperty给vm/组件对象添加与data中对应的属性
    - 在getter中读取data中对应属性返回
    - 在setter中将最新的value保存到data对应的属性上
  - 创建oberver(发布者):
    - 使用defineProperty来劫持/监视data中所有层次属性
    - 为data中每个属性创建对应的dep对象(订阅器)==>用于后面界面更新
  - 创建compile
    - 编译模板,实现界面的初始化显示
    - 为每个包含非事件指令表达式的节点创建对应的watcher
      - 绑定用于更新界面的回调函数
      - 将watcher(订阅者)添加到dep(订阅器)中去
- 更新数据后的基本流程
  - this.xxx = value
  - 由于有数据代理, data中的xxx会更新
  - 由于有数据劫持, xxx对应的setter就会调用
  - 在setter中,通过dep去通知所有对应的watcher去更新对应的节点

### 3)Vue双向数据绑定

- 通过v-model来实现双向数据绑定
- v-model的本质
  - 将动态的data数据通过value属性传给input显示==> data到view的绑定
  - 给input标签绑定input监听,一旦输入改变读取最新的值保存到data对应的属性上==> view到data的绑定
- 双向数据绑定是在单向数据绑定(data-->view)的基础,加入input事件监听(view ==> data)

## 响应式原理

- 初始化
  - 实现数据代理
    - 通过defineproperty给vm定义与data中属性对应的带getter/setter的属性
    - 在getter中,读取data中对应的属性值返回==>读取this.msg ==>读取的是data中msg属性值
    - 在setter中,将最新值保存到data对应的属性上==>this.msg = 'abc' ==> 'abc'会保存到data的msg上
  - 创建observer
    - 目标:对data中所有层次的属性进行监视/劫持
    - 通过defineproperty给data中所有层次属性,都重新定义,加上getter与setter
      - getter:用来建立dep与watcher的关系
      - setter:用来当data数据发生改变去更新界面
    - 为data中所有层次的属性创建一个对应的dep ==>用来将来更新界面的
  - 创建compile
    - 目标1:实现界面的初始化显示
    - 目标2:为将更新做准备
      - 为模板中每个包含表达式(事件表达式除外)的节点创建一个对应的watcher
      - 给watcher绑定用于更新对应节点的回调函数
      - 将watcher添加到n个对应的dep中
- 更新
  - this.msg = 'abc'
  - 由于有数据代理==> data的msg更新为了'abc'
  - 由于有数据劫持==>data中msg的setter调用了
  - 在setter中,通过对应的dep去通知所对应的watcher去更新对应的节点︰==>使用了订阅发布模式

## 可复用性

### 1)mixin(混入)

- 用来复用多个组件中相关的js代码的技术
- 将多个组件相同的js代码提取出来,定义在一个mixin中配置对象
- 在多个组件中通过mixins配置引入mixin中的代码,会自动合并到当前组件的配置中

### 2)自定义指令

```
vue.directive( 'upper-text', (e1，binding) => {
	e1.innerText = binding.value.touppercase()
})
```

### 3)自定义过滤器

```
//注册全局过滤器
vue.fi 1ter ('date-format', (value) => {
return moment(value).format( 'YYYY-MM-DD HH : mm : ss ')})
```

### 4)自定义插件

```
//对象插件
const myP1ugin = {
	insta11 (vue) {
	//通过Vue来扩展新的功能语法，如注册全局组件/指令/过滤器/ ...
	}
}
//函数插件
const myPlugin = (vue) => {
	//通过ue来扩展新的功能语法，如注册全局组件/指令/过滤器/ ...
}
export defau1t myPlugin
//在入口JS中引入，并声明使用来安装插件
import myplugin from './vue-myPlugin'
vue.use(myPlugin)
```

#### 问题: Vue.use()内部做了什么?

- 对象插件：调用插件对象install方法(传入Vue)来安装插件(执行定义新语法的代码)
- 函数插件：直接将其作为install来调用(传入Vue)来安装插件(执行定义新语法的代码)

## Vue状态管理: Vuex

### 1).vuex的5大属性

- state
- mutations
- actions
- getters
- modules

### 2)vuex的数据流结构图

![image-20210914002556495](.\media\4.png)

```
state:{

	product: {}
}
mutations:{
	RECEIVE_PRODUCT(state, product){
		state.product = product
	}
}
actions: {

	//获取商品的方法
	async getproduct({commit}){
        const result = await getProduct() //调用api函数
        commit('RECEIVE_PRODUCT',result.data)
	}，
	//删除商品
	asynnc deleteProduct({dispatch,commit,},id){
		const result = await delProduct(id)
		//成功获取失败的判断
		if(resu1t.code===200){
			//成功了,重新获取一次商品信息数据
			dispatch( 'getProduct')
		}e1se{
            //失败了
            throw错误
		}
	}
}

```

### 3)vuex多模块编程

- vuex的多模块编程的必要性
  - vuex单模块问题:
    - 需要的管理状态数据比较多，那对应的mutations/actions模块就会变得比较大
    - 如果添加新的数据管理,需要修改现在文件(不断向其添加内容)
  - vuex多模块编程:对各个功能模块的数据分别进行管理,这样更加具有扩展性
- 什么时候需要用vuex多模块编程?需要vuex管理的数据比较多时使用
- 多模块编程的总state结构:

```
{
	home:{
        categoryList: [],
        xxx:{}
	},
	user:{
		userInfo:{}
	}
}
```



### 4)问题1: vuex中的mutation可以执行异步操作吗?

- 可以==>异步更新数据后界面确实会自动更新
- 问题==>Vuex的调用工具监视不到mutation中的异步更新,工具记录还是更新前的数据(不对)
- 扩展:工具如何记录数据变化?==->每次mutation函数执行完后,立即记录当前的数据==>在mutation中同步更新state,才能被记录到

### 5)问题2: vuex中的状态数据的响应式的原理?

​	1.创建了一个vm对象

​	2.state中的数据都是vm的data数据(是响应式的)

​	3.组件中读取的state数据本质读取的就是data中的数据

​	4.一旦更新了state中的数据,所有用到这个数据的组件就会自动更新

### 6)问题3: vuex数据刷新丢失的问题

- 绑定事件监听:在卸载前保存当前数据

```
window.addEventListener('beforeun1oad',()=>{
    sessionstorage.setItem( 'CART_LIST_KEY',
    	JSON,stringify(this.$store.state.shopcart.cartList))
})
```

- 在初始时读取保存数据作为状态的初始值

```
cartList: JSON.parse(sessionstorage.getItem('CART_LIST_KEY')) || []，
```

## Vue路由: vue-router

### 1)一些基本知识

- 跳转/导航路由的2种基本方式
  - 声明式路由: <router-link to="/xxx" replace>xxx</router-link/>或者a标签
  - 编程式路由: this.$router.push(location)或者replace
- 跳转路由携带参数(数据)的方式
  - params
    - 注册的路由路径得有点位:/xxx/:name/:age？？参数可传可不传
    - 跳转时指定参数值:
      - /xxx/abc/12
      - {name: 'xxx', params: {name: 'abc', age: 123}}
  - query参数
    - ?后面的参数
    - 注册路由时不需要做特别的指定
  - props
    - props: true, //只能同名映射params参数
    - props: {a: 1, b : 'abc'},//只能映射非params/query参数
    - props: route =>({keyword3: route.params.keyword, keyword4: route.query.keyword2,xxx: 12}),//可以指定任何数据都可以
  - meta可以传递参数
- location的2种类型值
  - 字符串path
  - 对象形式: {name, path, params, query}
  - push(location)
  - <router-link :to="{}">

### 2)参数相关问题

- params与path配置能不能同时使用

  ​	不可以: router.push({path: '/xx', params: {name: 'tom'}})

  ​	params只能与name配合: router.push({name: 'xx', params: {name: 'tom'}})

- 如何配置params参数可传可不传?

  path: '/search/:keyword?",

  注意:一旦声明可以不传,不能传入一个空串的param参数

- 跳转携带的参数,刷新就丢失了

  如果注册没有指定/:xxx的点位,而跳转时通过params配置携带的参数数据,刷新时就会丢失

  因为url中没有携带的参数数据路径

- 路由组件能不能传递props参数?
  可以,但只是将params/query映射成props传入路由组件的

  #### 编程式路由跳转到当前路由,参数不变,会报出错误?==>在做项目时有没有遇到比较难/奇怪的问题?

  - 说明情况:

    - 上一个项目这种操作没有这个问题
    - 后面的一个项目(2019.8之后)开始有这个问题,而且是声明式跳转没有,只有编程式跳转有

  - 当编程式跳转到当前路由且参数数据不变,就会出警告错误:

    错误: Avoided redundant navigation to current location ==>重复跳转当前路由

  - 原因:

    vue-router在3.1.0版本(2019.8)引入了push()的promise的语法,如果没有通过参数指定回调函数就返回一个promise来指定成功/失败的回调,且内部会判断如果要跳转的路径和参数都没有变化,会抛出一个失败的promise说明文档: [https:/lgithub.com/vuejs/vue-router/releases?after=v3.3.1]()

  - 解决:

    - 办法1:在每次push时指定回调函数或catch错误

      ```
      push('/xxx'，()=→>{})===>声明式路由跳转本质就是这样执行的
      push('/xxx ').catch()
      ```

      

    - 

    - 办法2:重写VueRouter原型上的push方法(比较好)

      - 1).如果没有指定回调函数,需要调用原本的push()后catch()来处理错误的promise
      - 2).如果传入了回调函数,本身就没问题,直接调用原本的push()就可以

      ```
      const originPush = VueRouter.prototype.push
      VueRouter.prototype.push = function (1ocation，oncomplete，onAbort){
      	conso1e.log('push(', oncomplete，onAbort)
      	//判断如果没有指定回调函数，通过ca11调用源函数并使用catch来处理错误
          if (oncomp1ete===undefined && onAbort===undefined) {
      		return originPush.call(this,1ocation).catch(() =>{})
      	}e1se { //如果有指定任意回调函数，通过call调用源push函数处理
      		return originPush.ca11(this，1ocation，oncomplete，onAbort)
      	}
      }
      ```

      

      - 说明:
        声明式路由跳转之所有没有问题,是因为默认传入了成功的空回调函数

### 3)有点难度,但很重要的

- 路由懒加载:
  - () => import('./Home.vue')
  - 组件单独打包,开始不加载其打包文件,第一次请求时才会加载==>加载更快,提高用户体验
- 缓存路由组件

```
<keep-alive>
<router-view/>
</keep-alive>
```

​			路由离开时不销毁,路由回来时不用重新创建==>利用缓存,切换路由更快

​			再利用上prefetch实现预获取,用户体验更佳

- 动态添加路由

  - router.addRoutes(routes)
  - 在异步确定用户的权限路由后,需要动态添加到路由器

- 路由守卫与权限校验

  - router.beforeEach()注册全局前置守卫
  - 统一对用户权限进行一系列的校验处理

- history与hash路由的区别和原理

  - 区别：

    - history:路由路径不#,刷新会携带路由路径,默认会出404问题,需要配置返回首页

      - 404:

        - history有:刷新请求时会携带前台路由路径,没有对应的资源返回
        - hash没有:刷新请求时不会携带#路由路径

      - 解决:

        - 开发环境:如果是脚手架项目本身就配置好
          ==> webpack ==> devServer: {historyApiFallback : true}
          当使用HTML5 History APl时,所有的404请求都会响应index.html的内容

        - 生产环境打包运行:

          - 配置nginx

            ```
            1ocation / {
            	try_files $uri $uri/ /index.htm1; #所有404的请求都返回index页面
            }
            ```

    -  hash:路由路径带#,刷新不会携带路由路径,请求的总是根路径,返回首页,没有404问题

  - 原理:

    -  history:内部利用的是history对象的pushState()和replaceState()(H5新语法)
    - hash:内部利用的是location对象的hash语法
      - 写hash路径
      - 读hash路径
      - 监视hash路径的变化

- 如何让路由跳转后,滚动条自动停留到起始位置?

  ```
  new VueRouter({ //配置对象
  	// ...
  	scrollBehavior (to，from，savedPosition) {
  		//指定路由跳转后滚条的坐标
  		return { x: o，y: o }
  	}
  })
  
  ```

- 如何实现登陆后,自动跳转到前面要访问的路由界面

  - 在全局前置守卫中,强制跳转到登陆页面时携带目标路径的redirect参数

    ```
    if (userInfo.name) {
    	next()
    } e1se {
    	//如果还没有登陆，强制跳转到1ogin
    	next('/login?redirecf='+to.path) //携带目标路径的参数数据
    }
    ```

  - 在登陆成功后,跳转到redirect参数的路由路径上

    ```
    await this.$store.dispatch('login'，{mobile，password})
    //成功了，跳转到redirect路由或首页
    const redirect = this.$route.query.redirect
    this.$router.replace(redirect || '/')
    ```

### 4)路由导航守卫的理解和使用

- 导航守卫是什么?

  - 导航守卫是vue-router提供的下面2个方面的功能
    - 监视路由跳转-->回调函数
    - 控制路由跳转-->放行/不放行/强制跳转到指定位置 next()
  - 应用
    - 在跳转到界面前,进行用户权限检查限制(如是否已登陆/是否有访问路由权限)
    - 在跳转到登陆界面前,判断用户没有登陆才显示

- 导航守卫分类

  - 全局守卫:针对任意路由跳转

    - 全局前置守卫

      ```
      router.beforeEach((to，from，next) => {l / ...
      }
      ```

    - 全局后置守卫
      router.afterEach((to, from)=>{})

  - 路由守卫

    - 前置守卫

      ```
      {
          path: '/foo',
          component: Foo,
          beforeEnter : (to,from,next) =>{}
      }，
      {
          path: '/xxx',
          component: Foo,
          beforeEnter: (to, from, next)=>{}
      }
      ```

  - 组件守卫:只针对当前组件的路由跳转

    - 进入

      ```
      beforeRouteEnter (to，from，next) {
      	// 在渲染该组件的对应路由被confirm前调用
      	// 不!能!获取组件实例`this`
      	// 因为当守卫执行前·组件实例还没被创建
      	next(vm => {
      	// 通过vm访问组件实例
      	})
      }，
      ```

      

    - 更新:
      beforeRouteUpdate (to, from, next){}

    - 离开
      beforeRouteLeave (to, from, next){}
