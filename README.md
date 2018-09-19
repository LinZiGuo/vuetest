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
