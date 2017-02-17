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




 






