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

注意：vue低版本中 data里面的items和computed里面可以一样，但是高版本，是不允许相同名称。有很多小伙伴踩到了这个坑，这里提醒学习的小伙伴，根据自己版本的不同，请修改代码。

5、v-text & v-html
我们已经会在html中输出data中的值了，我们已经用的是{{xxx}},这种情况是有弊端的，就是当我们网速很慢或者javascript出错时，会暴露我们的{{xxx}}。Vue给我们提供的v-text,就是解决这个问题的。我们来看代码：
<span>{{ message }}</span>=<span v-text="message"></span><br/>

如果在javascript中写有html标签，用v-text是输出不出来的，这时候我们就需要用v-html标签了。

<span v-html="msgHtml"></span>

双大括号会将数据解释为纯文本，而非HTML。为了输出真正的HTML，你就需要使用v-html 指令。

需要注意的是：在生产环境中动态渲染HTML是非常危险的，因为容易导致XSS攻击。所以只能在可信的内容上使用v-html，永远不要在用户提交和可操作的网页上使用。

6、v-on：绑定事件监听器
v-on 就是监听事件，可以用v-on指令监听DOM事件来触发一些javascript代码。
一、使用绑定事件监听器，编写一个加分减分的程序。
我们的v-on 还有一种简单的写法，就是用@代替。
<button v-on:click="jiafen">加分</button>
<button @click="jianfen">减分</button>

我们除了绑定click之外，我们还可以绑定其它事件，比如键盘回车事件v-on:keyup.enter,现在我们增加一个输入框，然后绑定回车事件，回车后把文本框里的值加到我们的count上。
绑定事件写法：
<input type="text" v-on:keyup.enter="onEnter" v-model="secondCount">

javascript代码：
 onEnter:function(){
     this.count=this.count+parseInt(this.secondCount);
}

7、v-model指令
v-model指令，我理解为绑定数据源。就是把数据绑定在特定的表单元素上，可以很容易的实现双向数据绑定。
一、我们来看一个最简单的双向数据绑定代码：
html文件：

<div id="app">
    <p>原始文本信息：{{message}}</p>
    <h3>文本框</h3>
    <p>v-model:<input type="text" v-model="message"></p>
</div>

javascript代码：

var app=new Vue({
  el:'#app',
  data:{
       message:'hello Vue!'
  }
 })

 二、修饰符
.lazy：取代 imput 监听 change 事件。
.number：输入字符串转为数字。
.trim：输入去掉首尾空格。

三、文本区域加入数据绑定
	
<textarea  cols="30" rows="10" v-model="message"></textarea>

四、多选按钮绑定一个值
<h3>多选按钮绑定一个值</h3>
<input type="checkbox" id="isTrue" v-model="isTrue">
<label for='isTrue'>{{isTrue}}</label>

五、多选绑定一个数组
       <h3>多选绑定一个数组</h3>
       <p>
            <input type="checkbox" id="JSPang" value="JSPang" v-model="web_Names">
            <label for="JSPang">JSPang</label><br/>
            <input type="checkbox" id="Panda" value="Panda" v-model="web_Names">
            <label for="JSPang">Panda</label><br/>
            <input type="checkbox" id="PanPan" value="PanPan" v-model="web_Names">
            <label for="JSPang">PanPan</label>
            <p>{{web_Names}}</p>
       </p>

六、单选按钮绑定数据
<h3>单选按钮绑定</h3>
<input type="radio" id="one" value="男" v-model="sex">
<label for="one">男</label>
<input type="radio" id="two" value="女" v-model="sex">
<label for="one">女</label>
<p>{{sex}}</p>

8、v-bind 指令
v-bind是处理HTML中的标签属性的，例如<div></div>就是一个标签，<img>也是一个标签，我们绑定<img>上的src进行动态赋值。
html文件：
<div id="app">
    <img v-bind:src="imgSrc"  width="200px">
</div>

在html中我们用v-bind:src=”imgSrc”的动态绑定了src的值，这个值是在vue构造器里的data属性中找到的。

js文件：
var app=new Vue({
    el:'#app',
    data:{
          imgSrc:'http://baidu.com/wp-content/uploads/2017/02/vue01-2.jpg'
     }
})

我们在data对象在中增加了imgSrc属性来供html调用。

v-bind 缩写
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>

绑定CSS样式

在工作中我们经常使用v-bind来绑定css样式：
在绑定CSS样式是，绑定的值必须在vue中的data属性中进行声明。
1、直接绑定class样式
<div :class="className">1、绑定classA</div>

2、绑定classA并进行判断，在isOK为true时显示样式，在isOk为false时不显示样式。
<div :class="{classA:isOk}">2、绑定class中的判断</div>

3、绑定class中的数组
<div :class="[classA,classB]">3、绑定class中的数组</div>

4、绑定class中使用三元表达式判断
<div :class="isOk?classA:classB">4、绑定class中的三元表达式判断</div>

5、绑定style
<div :style="{color:red,fontSize:font}">5、绑定style</div>

6、用对象绑定style样式
<div :style="styleObject">6、用对象绑定style样式</div>

var app=new Vue({
   el:'#app',
   data:{
       styleObject:{
           fontSize:'24px',
           color:'green'
            }
        }
})
