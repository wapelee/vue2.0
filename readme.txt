live-server使用
用npm进行全局安装
cnpm install live-server -g

在项目目录中打开
live-server

Vue2.0的内部指令:
1、v-if:
v-if:是vue 的一个内部指令，指令用在我们的html中。

v-if用来判断是否加载html的DOM，比如我们模拟一个用户登录状态，在用户登录后现实用户名称。

关键代码：
<div v-if="isLogin">你好，JSPang！</div>

2、v-show ：
调整css中display属性，DOM已经加载，只是CSS控制没有显示出来。

<div v-show="isLogin">你好：JSPang</div>

3、v-if 和v-show的区别：
v-if： 判断是否加载，可以减轻服务器的压力，在需要时加载。
v-show：调整css dispaly属性，可以使客户端操作更加流畅。

4、v-for指令 ：解决模板循环问题
v-for指令是循环渲染一组data中的数组，v-for 指令需要以 item in items 形式的特殊语法，items 是源数据数组并且item是数组元素迭代的别名。

一、基本用法：
模板写法
 <li v-for="item in items">
        {{item}}
</li>

js写法
var app=new Vue({
     el:'#app',
     data:{
         items:[20,23,18,65,32,19,54,56,41]
     }
})

二、排序
我们已经顺利的输出了我们定义的数组，但是我需要在输出之前给数组排个序，那我们就用到了Vue的computed:属性。
computed:{
    sortItems:function(){
          return this.items.sort();
    }
}
我们在computed里新声明了一个对象sortItems，如果不重新声明会污染原来的数据源，这是Vue不允许的，所以你要重新声明一个对象。
注意：声明新的对象要在v-for="item in items"将items改成sortItems
但是这个小程序还是有个小Bug的，现在我把数组修改成这样。
items:[20,23,18,65,32,19,5,56,41]
我们把其中的54修改成了5，我们再看一下结果，发现排序结果并不是我们想要的。
我们可以自己编写一个方法sortNumber，然后传给我们的sort函数解决这个Bug。
function sortNumber(a,b){
            return a-b
  }
  用法
  computed:{
    sortItems:function(){
    return this.items.sort(sortNumber);
    }
 }

 这是在工作中非常常用的，一定要记住。

 三、对象循环输出
    我们先定义个数组，数组里边是对象数据
    
students:[
  {name:'jspang',age:32},
  {name:'Panda',age:30},
  {name:'PanPaN',age:21},
  {name:'King',age:45}
]

在模板中输出
<ul>
   <li v-for="student in students">
       {{student.name}} - {{student.age}}
   </li>
</ul>

加入索引序号：
<ul>
  <li v-for="(student,index) in students">
    {{index}}：{{student.name}} - {{student.age}}
  </li>
</ul>
排序，我们先加一个原生的对象形式的数组排序方法：
//数组对象方法排序:
function sortByKey(array,key){
    return array.sort(function(a,b){
      var x=a[key];
      var y=b[key];
      return ((x<y)?-1:((x>y)?1:0));
   });
}
有了数组的排序方法，在computed中进行调用排序
sortStudent:function(){
     return sortByKey(this.students,'age');
}