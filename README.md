# vuetest
vue练习demo
第一天总结
1.MVC和MVVM的区别
	MVC是后台开发的概念，MVVM是前端开发的概念，其中VM是调度核心，提供双向数据绑定

2.学习了vue中最基本代码的结构
	1.导入vue包
	2.创建要控制的区域

3.插值表达式 v-cloak v-text v-html v-bind(缩写是:) v-on(缩写是@) v-model v-for v-if v-show

4.事件修饰符： .stop	.prevent	.capture	.self	.once

5.el 指定要控制的区域	data是个对象，指定了控制区域内要用到的数据	methods虽然带个s后缀，但是是个对象，这里可以自定义方法

6.在VM实例中，如果要访问data上的数据，或者要访问methods中的方法，必须带this

7.在v-for要会使用key属性(只接受string/number)

8.v-model只能应用于表单元素

9.在vue中绑定样式两种方式，v-bind:class	v-bind:style

10.v-if有更高的切换消耗，v-show有更高的初始渲染消耗。因此，如果需要频繁切换v-show比较好；如果在运行时条件不大可能改变v-if比较好



第二天总结
1.过滤器调用的格式	{{ name | 过滤器的名称}}
	过滤器的定义语法
	Vue.filter('过滤器的名称', function(data){})
	过滤器中的function，第一个参数已经被规定死了，永远都是过滤器管道符前面传递过来的数据
	全局过滤器定义在Vue实例外，局部过滤器定义在Vue实例内部
	filters:{ //定义私有过滤器	过滤器有两个条件【过滤器名称|处理函数】
	//过滤器调用的时候，采用的是就近原则，如果私有过滤器和全局过滤器 名称一致，这时候优先调用私有过滤器
		dateFormat:function(dateStr,pattern){}
	}
2.保留两位，不足两位前面补0	String.padStart(2,'0')
3.按键别名：.enter .tab .delete(捕获“删除”和“退格”键) .esc .space .up .down .left .right
	自定义全局按键修饰符
	Vue.config.keyCodes.f2(自定义名称) = 113(对应的代码值)
4.使用Vue.directive()定义全局的指令 v-focus
	其中参数1：指令名称，注意：在定义的时候，指令单名称前面，不需要加v-前缀,但是，在调用的时候，必须在指令名称前加上v-前缀进行调用
	参数2：是一个对象，这个对象身上，有一些指令相关的函数，这些函数可以在特定的阶段，执行相关的操作
	自定义指令
	diretives:{
		'fontweight':{
			bind:function(el,binding){...}
		}
	}
	函数简写
	Vue.directive('color-swatch',function(el,binding){
		el.style.background = binding.value
	})
	diretives:{
		'fontsize':function(el,binding){ //注意：这个function等同于把代码写到了bind和update中去

		}
	}
5.vue实例的生命周期
	生命周期钩子=生命周期函数=生命周期事件
	主要的生命函数分类：
	创建期间的生命周期函数
		beforeCreate:实例刚在内存中被创建，data和methods未初始化
		created：实例已在内存中创建，data和methods已创建，没有开始编译模板
		beforeMount:已编译模板，未挂载到页面
		mounted:已挂载到页面
	运行期间的生命周期函数
		beforeUpdate:data的状态值是最新的，但页面的数据是旧的，还没重新渲染DOM节点
		updated：已渲染
	销毁期间的生命周期函数
		beforeDestroy：实例仍可用
		destroyed：已销毁
6.vue-resource依赖于vue，导包在vue.js后
	Vue.http.get(...)
	getInfo(){
		//发起get请求
		this.$http.get(url,[options]).then(successCallback,errorCallback);
		//发起post请求
		this.$http.post(url,[body],[options]).then(successCallback,errorCallback);
		//发起jsonp请求
		this.$http.jsonp(url,[options]).then(successCallback,errorCallback);
	}




1.全局配置根域名 则，在每次单独发起HTTP请求时，请求URL路径，应该以相对路径开头，前面不能带/，否则不会启用根路径做拼接
	Vue.http.options.root = 'http://vue.studyit.io/'
2.全局配置emulateJSON选项
	Vue.http.options.emulateJSON = true
3.使用过渡类名实现动画
	使用transition元素，把需要被动画控制的元素包裹起来
	transition元素，是vue官方提供的
	使用duration="毫秒值" 来统一设置入场和离场时的动画时长
	在实现列表过渡时，如果需要过渡的元素，是通过v-for循环渲染出来的，不能使用transition包裹，需要使用transitionGroup
	如果腰围v-for循环创建的元素设置动画，必须为每一个元素设置:key属性
	给transition-group添加appear属性，实现页面刚展示出来时，入场的效果
	通过为transition-group元素，设置tag属性，指定transition-group渲染为指定的元素，如不指定，默认渲染为span标签
	<transition-group appear tag="ul">
		<li v-for="(item,i) in list" :key="item.id" @click="del(i)">
			{{item.id}} --- {{item.name}}
		</li>
	</transition-group>
	.v-move和.v-leave-active配合使用，能够实现列表后续元素渐渐飘上来的效果
	v-enter：还没有进入
	v-leave-to:已经离开
	v-enter-active:入场动画的时间段
	v-leave-active:离场动画的时间段
	<style>
		.v-enter,
		.v-leave-to{
			opacity:0;
		}
		.v-enter-active,
		.v-leave-active{
			transition:all 0.4s ease;
		}
		.v-move{
			transition:all 0.6s ease;
		}
		.v-leave-active{
			position:absolute;
		}
	</style>
	<transition enter-active-class="bounceIn" leave-active-class="bounceOut" :duration="{ enter:200, leave:400 }">
		<h3 v-if="flag">这是一个H3</h3>
	</transition>
4.模块化：从代码逻辑的角度划分的；方便代码分层开发，保证每个功能模块的职能单一
组件化：从UI界面的角度划分的；前端的组件化，方便UI组件的重用
5.如果要使用组价，直接把组件的名称，以HTML标签的形式，引入到页面中即可
<my-com1></my-com1>
5.1 使用Vue.extend来创建全局的Vue组件
var com1 = Vue.extend({
	template:'<h3>Vue.extend创建的组件</h3>' //通过Template属性，指定了组件要展示的HTML结构
})
5.2 使用Vue.component('组件名称',创建出来的组件模板对象)
如果使用Vue.component定义全局组件时，组件名称使用驼峰命名，则在引用组件时，需要把大写的驼峰改为小写字母同时加-连接
如果不使用驼峰命名，则直接引用
Vue.component('myCom1',com1)
6.Vue.component('mycom1',Vue.extend({
	template:'<h3>...</h3>'
}))
7.Vue.component('mycom1',Vue.extend({
	template:'#tem1'
}))
在被控制的#app外面，使用template元素，定义组件的HTML模板结构
<template id="tem1">
	<div>
		<h3>...</h3>
	</div>
</template>
8.定义局部组件
组件的data必须为方法，方法内部必须返回一个对象，保证不同实例互不影响
	components:{
		login:{
			template:'<h3>...</h3>'
			data:function(){
				return {}
			}
		}
	}
9.组件切换
	9.1 if-else
	<a href="" @click.prevent="flag=true">登录</a>
	<a href="" @click.prevent="flag=false">注册</a>
	<login v-if="flag"></login>
	<register v-else="flag"></register>
	9.2 Vue提供了component来展示对应名称的组件
	component是一个占位符，:is属性，可以用来指定要展示的组件的名称
	<component :is="'login'"></component>
	9.3 组件切换-应用切换动画和mode方式
	通过mode属性，设置组件切换时的模式
	<transition>
		<component :is="'login'"></component>
	</transition>
