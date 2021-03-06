<html>
<head>
</head>
<meta charset="utf-8">
<body>
	<h1 style="text-align: center">通过Jmeter上传文件</h1>
<div class="lemmaWgt-lemmaCatalog">
<div class="lemma-catalog">
<h2 class="block-title">目录</h2>
<div class="catalog-list column-1">
<ol>
	<li class="level1">
		<!--<span class="index">1</span>-->
		<span class="text"><a href="#1">背景简述</a></span>
	</li>
	<li class="level1">
		<!--<span class="index">2</span>-->
		<span class="text"><a href="#2">问题描述</a></span>
	</li>
	<li class="level1">
		<!--<span class="index">3</span>-->
		<span class="text"><a href="#3">解决方式</a></span>
	</li>
</ol>
</div>
</div>
</div>
<h2 class="title-text" id="1">一、背景简述：</h2>
<div class="para" label-module="para">
	<p style="text-indent: 2em">
	<font>在用jmeter测试文件上传接口的时候，发现jmeter发送<b>post</b>报文的时候，配置报文头字段有一些坑，特记录下来以备后用。</font></p>
</div>
<h2 class="title-text" id="2">二、问题描述：</h2>
<div class="para" label-module="para">
		<h3><p style="text-indent: 2em">按照网上的教程：</p></h3>
		<p style="text-indent: 2em">
		<ol>
			<li>
				创建线程组
			</li>
			<li>
				<p>创建http请求，并配置好参数。<br>
				包括ip，端口，报文方法<b>(post)</b>，路径，报文参数(即要上传文件在本地的路径和文件名，上传格式)。如图：<br>
				由于文件上传接口部在本机，所以发往本机。<br>
				<img src="./p/problem_1/jmeter_request_config.jpg" width="40%" alt="jmeter请求配置"></p>
			</li>
		</ol>
	</p>
		<p style="text-indent: 2em">
		在jmeter发送完报文之后，服务器端解析报文获取上传文件时报错"<font style="color: red">no multipart boundary param in Content-Type</font>"，jmeter端请求报文显示如下图：<br></p>
		<p style="text-indent: 2em">
		<img src="./p/problem_1/jmeter_request_1.jpg" width="40%" alt="jmeter请报文"><br>
		<p style="text-indent: 2em">
		发现报文请求头里不包含属性<b><font style="color:red">boundary</font></b>。</p>
</div>
<h2 class="title-text" id="3">三、解决方式：</h2>
<div>
	<p style="text-indent: 2em">
	经过查询，<b>boundary</b>是上传文件时，随机生成的字符串放在协议头里，用来分隔文本的开始和结束，如上图的<b>postData</b>里，<font style="color: red">6XvRd1YrORtf37_4bKpUg-Fps_M1ZIm4lCn</font>就是boundary的值。<br>
		<p style="text-indent: 2em">
			经过多次尝试以及查询网络，发现问题是在<b>jmeter</b>上配置报文头的时候，配置了<b>Content-Type</b>的缘故。在报文头里不配置<b>Content-Type</b>，jmter会自动根据报文内容来确定报文格式，并生成对应的boundary。去掉之后，再次发送就成功了。</p></p>
</div>
</body>
</html>