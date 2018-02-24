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

9、其他内部指令(v-pre & v-cloak & v-once)
v-pre指令
在模板中跳过vue的编译，直接输出原始值。就是在标签中加入v-pre就不会输出vue中的data值了。

<div v-pre>{{message}}</div>

这时并不会输出我们的message值，而是直接在网页中显示{{message}}

v-cloak指令
在vue渲染完指定的整个DOM后才进行显示。它必须和CSS样式一起使用，HTML 绑定 Vue实例，在页面加载时会闪烁,加上v-cloak可以解决

[v-cloak] {
  display: none;
}

<div v-cloak>
  {{ message }}
</div>

v-once指令
在第一次DOM时进行渲染，渲染完成后视为静态内容，跳出以后的渲染过程。

<div v-once>第一次绑定的值：{{message}}</div>
<div><input type="text" v-model="message"></div>

全局API：

1、Vue.directive 自定义指令
一、什么是全局API？
全局API并不在构造器里，而是先声明全局变量或者直接在Vue上定义一些新功能，Vue内置了一些全局API，说的简单些就是，在构造器外部用Vue提供给我们的API函数来定义新的功能。

二、Vue.directive自定义指令
自己定义一个全局的指令。我们这里使用Vue.directive( );
Vue.directive('jspang',function(el,binding,vnode){
        el.style='color:'+binding.value;
});

可以看到数字已经变成了绿色，说明自定义指令起到了作用。可能您看这个代码还是有些不明白的，比如传入的三个参数到底是什么。
三、自定义指令中传递的三个参数

el: 指令所绑定的元素，可以用来直接操作DOM。

binding:  一个对象，包含指令的很多信息。

vnode: Vue编译生成的虚拟节点。

四、自定义指令的生命周期

自定义指令有五个生命周期（也叫钩子函数），分别是 bind,inserted,update,componentUpdated,unbind

bind:只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个绑定时执行一次的初始化动作。
inserted:被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于document中）。
update:被绑定于元素所在的模板更新时调用，而无论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。
componentUpdated:被绑定元素所在模板完成一次更新周期时调用。
unbind:只调用一次，指令与元素解绑时调用。

五、解绑数据
app.$destroy()

2、Vue.extend构造器的延伸
一、什么是Vue.extend？
Vue.extend 返回的是一个“扩展实例构造器”,也就是预设了部分选项的Vue实例构造器。经常服务于Vue.component用来生成组件，可以简单理解为当在模板中遇到该组件名称作为标签的自定义元素时，会自动调用“扩展实例构造器”来生产组件实例，并挂载到自定义元素上。

由于我们还没有学习Vue的自定义组件，所以我们先看跟组件无关的用途。

二、自定义无参数标签
我们想象一个需求，需求是这样的，要在博客页面多处显示作者的网名，并在网名上直接有链接地址。我们希望在html中只需要写<author></author> ，这和自定义组件很像，但是他没有传递任何参数，只是个静态标签。

我们的Vue.extend该登场了，我们先用它来编写一个扩展实例构造器。代码如下：
var authorExtend = Vue.extend({
    template:"<p><a :href='authorUrl'>{{authorName}}</a></p>",
    data:function(){
    return{
          authorName:'JSPang',
          authorUrl:'http://www.jspang.com'
          }
    }
});

这时html中的标签还是不起作用的，因为扩展实例构造器是需要挂载的，我们再进行一次挂载。

new authorExtend().$mount('author');

这时我们在html写<author><author>就是管用的.

三、挂载到普通标签上
还可以通过HTML标签上的id或者class来生成扩展实例构造器，Vue.extend里的代码是一样的，只是在挂载的时候，我们用类似jquery的选择器的方法，来进行挂载就可以了。
new authorExtend().$mount('#author');