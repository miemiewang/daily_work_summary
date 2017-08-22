###2017.1.12
w 保存当前编辑文件但不退出，使用时可以再给编辑文件起一个新的文件名
:w newfile
如果newfile是一个已存在文件，仍要替换，可使用
:w! newfile
否则，输入
:q 编辑文件没有被保存，但并不退出，如果要强行退出，则输入
:q!放弃修改退出
:wq 先保存文件，然后退出

### 2017.2.10
#### 移动端所有设备实现1px边框效果方法：
html:
	
	<div class="line"></div>
css:

	.line{
		height:1px;
	}
	
	@media (-webkit-min-device-pixel-ratio:1.5),(min-device-pixel-ratio:1.5) {
        .line {
            border: none;
            background-image: -webkit-gradient(linear,left top,left bottom,color-stop(50%,transparent),color-stop(50%,transparent),color-stop(100%,@borderColor));
            background-image: -webkit-linear-gradient(top,transparent 50%,@borderColor 50%,@borderColor 100%);
            background-image: linear-gradient(to bottom,transparent 50%,@borderColor 50%,@borderColor 100%);
            background-size: 100% 2/3px;
            background-repeat: no-repeat;
            background-position: bottom;
        }
    }

    @media (-webkit-min-device-pixel-ratio:2),(min-device-pixel-ratio:2) {
        .line {
            border: none;
            background-image: -webkit-gradient(linear,left top,left bottom,color-stop(50%,transparent),color-stop(50%,transparent),color-stop(100%,@borderColor));
            background-image: -webkit-linear-gradient(top,transparent 50%,@borderColor 50%,@borderColor 100%);
            background-image: linear-gradient(to bottom,transparent 50%,@borderColor 50%,@borderColor 100%);
            background-size: 100% 1px;
            background-repeat: no-repeat;
            background-position: bottom;
        }
    }
    
注：如果头部viewport设置缩放，像素也会自动缩放，就不用通过渐变1像素了

###2017.2.17
####解决浮点数加减乘除运算的精度误差
 * 求和
 
		 function add(a, b) {
		    var c, d, e;
		    try {
		        c = a.toString().split(".")[1].length;
		    } catch (f) {
		        c = 0;
		    }
		    try {
		        d = b.toString().split(".")[1].length;
		    } catch (f) {
		        d = 0;
		    }
		    return e = Math.pow(10, Math.max(c, d)), (mul(a, e) + mul(b, e)) / e;
		}
		
* 相减
	
			function sub(a, b) {
			    var c, d, e;
			    try {
			        c = a.toString().split(".")[1].length;
			    } catch (f) {
			        c = 0;
			    }
			    try {
			        d = b.toString().split(".")[1].length;
			    } catch (f) {
			        d = 0;
			    }
			    return e = Math.pow(10, Math.max(c, d)), (mul(a, e) - mul(b, e)) / e;
			}
			
* 相乘
		
				function mul(a, b) {
				    var c = 0,
				        d = a.toString(),
				        e = b.toString();
				    try {
				        c += d.split(".")[1].length;
				    } catch (f) {}
				    try {
				        c += e.split(".")[1].length;
				    } catch (f) {	    			return Number(d.replace(".", "")) * Number(e.replace(".", "")) / Math.pow(10, c);
				}

 	* 相除

	 		  	function div(a, b) {
				    var c, d, e = 0,
				        f = 0;
				    try {
				        e = a.toString().split(".")[1].length;
				    } catch (g) {}
				    try {
				        f = b.toString().split(".")[1].length;
				    } catch (g) {}
				    return c = Number(a.toString().replace(".", "")), d = Number(b.toString().replace(".", "")), mul(c / d, Math.pow(10, f - e));
				}

###2017.2.20
命令：lh 机器名 查看机器tag
###2017.2.23
将远程开发机文件拷到本地目录 
在本地执行：scp -r wangyang@10.4.21.32:repos/ssad/webroot/resource/pgcpromotion_mobile ~/Desktop/
###2017.2.27
判断是否为空对象

		function isEmptyObj(obj) {
            for (var name in obj) {
                return false;
            }
            return true;
        }
        
###2017.3.9
####滚动到底部加载问题：

#####1.全局滚动

		window.onscroll = function() {
			var bHeight = document.body.clientHeight, // body对象高度,如果有滚动高度也包括
			    wHeight = window.innerHeight, // 浏览器窗口的视口
			    sTop = document.body.scrollTop; // body距离滚动顶部的距离
			var isScrollBottom = bHeight - (wHeight + sTop) === 0 ? true : false;
			if (isScrollBottom) {
				执行加载
			}
		}
#####2.局部滚动

		document.getElementById('div1').onscroll = function() {
			var sHeight = this.scrollHeight， // 元素内容高度，包括overflow:hidden不可见内容
			    dHeight = this.clientHeight || this.offsetHeight || this.getBoundingClientRect().height， // 元素内容高度，不包括overflow:hidden不可见内容
			    sTop = this.scrollTop; // 元素距离滚动顶部的距离
			var isScrollBottom = sHeight - (dHeight + sTop) === 0 ? true : false;
			if (isScrollBottom) {
				执行加载
			}
		}
注意：移动端下拉加载时，全局加载会有问题，所以一般绑定到具体元素上执行局部加载

####获得元素内容的行数：

	var H = div.clientHeight || div.offsetHeight;
	var Line = div.currentStyle ? div.currenStyle['line-height'] : window.getComputedStyle(div, null)['line-height'];
	H/parseFloat(Line) // 行数
	
####-webkit-line-clamp 限制在一个块元素显示的文本的行数。 为了实现该效果，它需要组合其他外来的WebKit属性。常见结合属性：

* display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
* 	-webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。
* text-overflow，可以用来多行文本的情况下，用省略号“...”隐藏超出范围的文本 。

超出部分以省略号表示：

			可以设置多行
			-webkit-line-clamp: 1;
			text-overflow: ellipsis;
			overflow: hidden;
			display: -webkit-box;
			-webkit-box-orient: vertical;
			单行
			white-space: nowrap;
			text-overflow: ellipsis;
			overflow: hidden;
		
###2017.3.13
####git学习笔记
git status -s 获得简短的输出
AM前缀表示将文件添加到缓存后又有新的改动
git checkout file 表示文件还未添加到缓存区时撤销修改
git reset HEAD file 表示撤销已经添加到缓存区的文件
git rm file 将文件从缓存区和工作目录中移除，彻底删除
git branch --track [branch] [remote-branch] 新建一个分支，并与指定远程分支建立追踪关系
git log --stat 显示commit历史以及每次commit发生变更的文件
###2017.3.19
####移动端踩坑总结
#####1.软键盘的类型
input输入框调起软键盘类别根据input的type决定，input[type=tel],input[type=number]可调起数字面板，但是在ios系统下有bug,需要加正则pattern='[0-9]*'或pattern="/d*"判断；当input[type=tel]时，安卓下仍然可以输入非数字类型
#####2.不同系统中软键盘调起时的特点

* iOS系统  
a.如果控件在键盘高度上方的话,键盘是以一个浮层的方式弹出,并且将那个触发的控件推到键盘的上方.  
b.如果控件在页面底部,键盘会覆盖该控件,系统会将整个页面向上推,直到将那个控件推到键盘上方为止.
* Android系统  
a.有一部分的Android的实现和iOS一样.(未验证）  
b.另一部分，如果发现触发的input控件比键盘的高度低的时候，会自动将整个document的高度增加，增加到这个控件的高度超过键盘的高度为止.(未验证）  
c.还有一部分,如果触发的input控件比键盘低，键盘会覆盖到控件上从而挡住控件

解决方法：

* ios系统
如果ios系统下页面可滚动，最好将滚动设为局部滚动，这样可以避免键盘弹出时，控件会随着页面滚动，定位失效
* Android系统
由于安卓下键盘可能挡住控件，所以调起键盘时(input的focus事件)，需要将控件定位到页面最上方，使键盘尽量不挡住控件

###2017.3.23
####shell命令
#####1.source 命令是 bash shell 的内置命令，从 C Shell 而来。source 命令的另一种写法是点符号，用法和 source 相同，从Bourne Shell而来。
    source 命令可以强行让一个脚本去立即影响当前的环境。
    source 命令会强制执行脚本中的全部命令,而忽略文件的权限。
    source 命令通常用于重新执行刚修改的初始化文件，如 .bash_profile 和 .profile 等等。
    source 命令可以影响执行脚本的父shell的环境，而 export 则只能影响其子shell的环境。
    
注：刚修改的文件需要source一下才能生效，如根目录下的.bashrc

###2017.7.18

* /etc/profile、/etc/bashrc、/etc/paths、~/.bash_profile 、~/.bash_login、 ~/.profile、~/.bashrc、~/.zshrc

 	* /etc/profile和/etc/paths是系统级别的，系统启动就会加载。/etc/profile中设定的变量(全局)的可以作用于任何用户,而~/.bashrc等中设定的变量(局部)只能继承/etc/profile中的变量,他们是"父子"关系.  
 
 	* /etc/bashrc:为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取.   
 
 	* ~/.bash_profile 、~/.bash_login、 ~/.profile是当前用户级的环境变量。后面3个按照从前往后的顺序读取，如果~/.bash_profile文件存在，则后面的几个文件就会被忽略不读了   

 	* ~/.bash_profile 是交互式、login 方式进入 bash 运行的。~/.bashrc 是交互式 non-login 方式进入 bash 运行的。通常二者设置大致相同，所以通常前者会调用后者
 		
* 要想让python解释器找到自己编写的模块，则该模块必须PYTHONPATH上，否则在导入该模块时会出现找不到该模块的错误。想要python去哪个目录下找模块，就设置到哪个目录下。例如在.bashrc里定义：

		export PYTHONPATH=~/repos
		
###2017.7.26

toString() 方法可把一个 Number 对象转换为一个字符串 
 
		Number(123).toString() // '123'
		new Number(123).toString() // '123'
		123.toString() // false
		
千分位格式化
		
			function formatNumber(num) {
				  if (isNaN(num)) {
				    throw new TypeError("num is not a number");
				  }
				
				  var groups = (/([\-\+]?)(\d*)(\.\d+)?/g).exec("" + num),
				    mask = groups[1],            //符号位
				    integers = (groups[2] || "").split(""), //整数部分
				    decimal = groups[3] || "",       //小数部分
				    remain = integers.length % 3;
				
				  var temp = integers.reduce(function(previousValue, currentValue, 						index) {
				    if (index + 1 === remain || (index + 1 - remain) % 3 === 0) {
				      return previousValue + currentValue + ",";
				    } else {
				      return previousValue + currentValue;
				    }
				  }, "").replace(/\,$/g, "");
				  return mask + temp + decimal;
			}






	




 






