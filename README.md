lib-qqwry
=====

lib-qqwry是一个高效查询纯真IP库(qqwry.dat)的模块;  
为更好的转化效率,抛弃了iconv-lite模块,直接使用gbk编码表文件。  
经过不断优化，相同环境下，单次查询速度从最初的0.6毫秒提升到现在的0.004毫秒;  

### 实现的功能
1.通过 IP地址/或有效的IP数值 搜索IP地址的地理位置。  
2.搜索一个IP段的地理位置信息。  
3.IP地址与数值的互转。  

### npm安装
<pre>
npm install lib-qqwry
</pre>

### 调用方法
<pre>
var qqwry = require('lib-qqwry').info(); //调用并初始化，不在乎70毫秒左右初始化时间，完全可以这样调用;
var ipL = qqwry.searchIP("202.103.102.10"); //查询IP信息
var ipLA = qqwry.searchIPScope("0.0.0.0","1.0.0.0");  //查询IP段信息
</pre>

## API

### info(dataPath,callback) IP库初始化
dataPath : IP库路径,可选; //默认路径为主文件同目录下(__dirname + "/qqwry.dat");   
callback : 回调函数 //可在此时调用查询函数  

<pre>
//你可以这样
qqwry.info();
var ipL = qqwry.searchIP("202.103.102.10");

//也可以这样初始化,推荐;
qqwry.info(function(){
	var ipL = qqwry.searchIP("202.103.102.10");
});
</pre>

### infoAsync(dataPath,callback) IP库初始化的异步方法
info()的异步方法；
初始化需要70毫秒，以及占用9MB左右的内存，项目资源紧张可以异步初始化。

### SearchIP(IP) 单个IP查询
IP : IP地址  
反回一个JSON对像;  
<pre>
> qqwry.searchIP("255.255.255.255");
{ ip: '255.255.255.255',
  Country: '纯真网络',
  Area: '2013年6月10日IP数据' }
</pre>

### searchIPScope(beginIP,endIP) IP段查询
beginIP : 启始IP  
endIP : 结束IP  
反回一个JSON对像数组;  
<pre>
> qqwry.searchIPScope("0.0.0.0","1.0.0.0");
[ { begIP: '0.0.0.0',
    endIP: '0.255.255.255',
    Country: 'IANA保留地址',
    Area: ' CZ88.NET' },
  { begIP: '1.0.0.0',
    endIP: '1.0.0.255',
    Country: '澳大利亚',
    Area: ' CZ88.NET' } ]
</pre>

### searchIPScopeAsync(beginIP,endIP,callback) IP段查询的异步方法
searchIPScope() 的异步方法,查询结果会以第一个参数的形式传给回调函数;  

### ipToInt(IP) IP地址转数值
<pre>
> qqwry.ipToInt("255.255.255.255")
4294967295
</pre>

### intToIP(INT) 数值转IP地址
<pre>
> qqwry.intToIP(4294967295)
'255.255.255.255'
</pre>

## 文档说明
1. index.js 解析IP库的主文件;
2. gbk.js 	GBK编码表文件,从(iconv-lite)中提取出来的,只增加了一个转码方法;
3. test.js	调用演示;
4. test_v.js 效率测试示例;
5. qqwry.dat 纯真IP库,可用最新IP库替换;

### 效率测试文件 test_v.js
`node test_v.js 255.255.255.255` 正常工作检查  
`node test_v.js -1` 单个查询效率测试  
`node test_v.js -2` IP段查询效率测试  

##作者
[含浪](http://www.cnblogs.com/whyoop)   w.why@163.com


