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
