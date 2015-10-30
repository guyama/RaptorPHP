
#关于RaptorPHP
-------------
迅猛龙PHP框架，简称*RP*框架  
小巧、高效、安全，据说用了以后RP会变好 ;)    
兼容新浪SAE，为移动端特别优化  
项目主页：http://www.raptorphp.com  
Github主页：http://www.github.com/raptorphp/  


#安装
-------------
##环境要求
* PHP 5.3.2+ 
* Mysql：5.0+
* 兼容新浪sae
* php组件要求
	* PDO：数据库连接
	* MB系列函数（*可选）：汉字判断
	* CURL：微信等api


#快速上手


##项目配置
* 在app目录下，按照模板创建项目文件夹，例如mycms，目录名全小写
* 在项目目录下的"config/"中配置文件，配置参考模板示例
* 把域名指向你的项目文件夹
* 把静态文件的二级域名指向app/static/项目名/  例：http://i.78.com  指向 app/static/78/



#开发文档
-------------
* 创建模块文件夹
* 设置入口文件
* 生成二维码
SAE 官方基于开源的一种二维码实现封装了一个二维码的SDK，目前已经开源在GitHub，https://github.com/sinacloud/qrcode。目前只支持PHP RUNTIME.

##开发规范

###标记
* 所有php文件，其代码标记均采用完整php标签，不使用短标签
* 对于只有php的代码文件，建议省略结尾处的'?>',防止多余的空格或其他字符影响到代码。

###目录结构
raptor/  RP框架主目录  
raptor/core/  RP框架核心类库
raptor/extend/ RP框架扩展类库

app/ 项目总目录  
app/appname/  自定义项目目录
app/appname/index.php 项目入口文件  
 
app/static/ 网站静态文件主目录 
app/static/appname/ 项目静态文件目录  
app/static/appname/common/ 项目通用静态文件
 
> 全部目录`不可写`，兼容新浪SAE，保障安全性

###命名空间：

####规范
* 命名空间层级与文件夹一致,但文件夹名全小写
* 采用首字母大写或者PASCAL命名约定，不可使用保留词
* 在命名空间中访问全局类时，需要加斜线，例：namespace A\B\C;  $b = new \Exception('hi'); // $b 是类 Exception 的一个对象
* 使用"未定义空间"即"全局空间"下的函数时，需要加斜线，例：  $f = \fopen(...); // 调用全局的fopen函数
* 同一个命名空间可以定义在多个文件中，即允许将同一个命名空间的内容分割存放在不同的文件中
* 非常不提倡在同一个文件中定义多个命名空间。这种方式的主要用于将多个 PHP 脚本合并在同一个文件中
* 手工实例化时要使用单引号，否则可能会引起转译问题，例：$a = new "dangerous\name"; // \n 会被转译成换行符


####Raptor命名空间结构
\Raptor  
\Raptor\Core  
\Raptor\Extend  

\Appname  
\Appname\Model  
\Appname\Control  


###文件名
均以.php结尾
model类：xxx.class.php
control类：xxx.control.php

###变量
* 类型缩写（小写）＋描述（首字母大写）
* 类型包括：s=>string，i=>int, ar=>array , c=>class , o=>object， tmp=>temporary 
* 例：$sTitle 表示字符串的标题，$arError 表示数组错误信息

###函数名

###类名

###方法名

###字段
字段尽可能用NOT NULL，因为NULL占空间，且不进入索引，且查询语句要用IS NULL


###SQL 语句
SQL 关键字永远使用大写：SELECT、INSERT、UPDATE、WHERE、AS、JOIN、ON、IN 等。



##调试

> 调试（debug）模式尽量不要在生产环境开启

###开启debug模式
在项目入口文件中，设置APP_DEBUG为TRUE|FALSE，开启|关闭debug模式（*尽量不要在生产环境开启* )   

###设置debug模式
在项目入口文件中，设置APP_DEBUG_MODE的为 normal|brower  模式
normal模式：直接在页面显示错误
browser模式：在浏览器Console面板输出错误（目前版本仅支持Chrome）

> 在Chrome浏览器调试出错信息：本系统使用ChromePHP插件，项目主页：https://github.com/ccampbell/chromephp

###自动捕捉错误：
E_ERROR、E_PARSE、E_CORE_ERROR、E_CORE_WARNING、 E_COMPILE_ERROR、E_COMPILE_WARNING  以外级别的错误，将被系统自动捕捉并按指定的各式显示

###抛出异常：
throw new Exception('自定义异常信息');

###各种调试的方法
*方法一：exit();  die(); *
一般用于手工调试，不要在生产环境中使用。

*方法二：thorw new Exception('异常信息')*

程序执行过程中出现意料之外的情况，逻辑上往往是行得通的，但不符合应用场景，比如接收到一个长度超出预定格式的用户命名，因此，异常主要靠编码人员做预先做判断后抛出，捕获异常后改变程序流程来处理这些情况，不必中断程序。


需要立即中断执行的场合，例如：  
数据库连接错误


*方法三：trigger_error(); * 
不会中断运行，使用场合：



###性能调试
Debug::memUse(); //显示当前的服务器内存消耗
* 在需要开始统计执行的地方放置：Debug::runInfo('start'); 在需要结束统计执行信息的地方放置：Debug::runInfo('end'); 
* 显示统计信息用：echo Debug::runInfo('end');
* 如果不设置start，则在显示程序从一开始运行到现在的信息。
* 如果在SAE环境下，可在SAE后台的XHProf面板中查看信息



##引用
类文件放在指定的目录下，Raptor就会按需自动加载  
例：
*方式1：*
```php
\Raptor\Core\Db::select();  会自动加载  raptor/core/db.class.php  
```
*方式2：*(仅限于raptor/core/核心类)
```php
use Raptor\Core\Db;  
Db::select();  
```

##数据操作


###查询
*pdo模式*
```php
\Raptor\Core\Db::select( 'SELECT * FROM `article` WHERE `isdel` = :isdel',array('isdel'=>'1') );   //返回结果数组  （只能执行select）
```
*连贯操作模式*
```php
\Raptor\Core\Db::select( 'id,title' ) //查询的字段，留空则默认查询全部
				->table( 'article' )  //表名
				->where( 'isdel','!=','1' )  //字段、操作符、值
				->where( 'title','IS NULL' )// 查询为NULL的记录（不提倡字段用NULL）
				->orWhere( 'title','LIKE','%test%' )   //LIKE操作，百分号直接写在值里
				->orderby( 'id DESC,orderid ASC' ) //排序
				->limit(20)  // ->limit(0,20)   
				->get(); //执行，无参数
```			
> 返回结果数组



###插入

*pdo模式1*
```php
\Raptor\Core\Db::insert( 'article' , array('title'=>'test','test'=>'test2' ) );  //传入表名和插入值数组
```
*pdo模式2*
```php
\Raptor\Core\Db::insert('INSERT INTO `article` (`title`,`isdel`) VALUES (:title,:isdel)',array('title'=>'test','isdel'=>'0'));  //传入完整insert sql和插入值数组
```
> 返回插入记录的id


###更新
*pdo模式*
```php
\Raptor\Core\Db::update( 'UPDATE `article` SET `title`=:title,`test`=:test WHERE `id` = :id AND `isdel` = :isdel' , array( 'title'=>'te55522','test'=>'hehe','id'=>'5','isdel'=>'0' ) );   
 ```

*连贯操作模式*
```php
\Raptor\Core\Db::update()
				->table('article')
				->set( 'title','tefucst2' )
				->set( 'test','yes2' )
				->where( 'id','=','5' )
				->get(); 
```
*自增模式*
```php
Db::update()->table( 'article' )->increase( 'click' );
Db::update()->table( 'article' )->increase( 'click' , 5 );
```
*自减模式*
```php
Db::update()->table( 'article' )->decrease ( 'click' );
Db::update()->table( 'article' )->decrease ( 'click' , 5 );
```

> 返回影响结果数，更新值和旧的一样时，结果数为0



###删除
*pdo模式*
```php
\Raptor\Core\Db::delete( 'DELETE FROM `article` WHERE `isdel` = :isdel',array('isdel'=>'1') ); 
```

*连贯操作模式*
```php
\Raptor\Core\Db::delete() //查询的字段，留空则默认查询全部
				->table('article')  //表名
				->where('isdel','!=','1')  //字段、操作符、值
				->orWhere('title','LIKE','%test%')   //LIKE操作，百分号直接写在值里
				->get(); 
```

> 返回影响结果数
> 重要业务模块，尽量不要使用物理删除数据，而使用isdel字段标示。



###原生表达式
```php
Db::raw();
```
> 原生表达式没有经过pdo的prepare过滤，存在注入风险，要谨慎使用。








