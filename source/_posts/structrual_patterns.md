---
title: PHP设计模式之结构型模式（structural patterns）
tags: 设计模式
categories: 设计模式
---
### 适配器模式
>涉及到一个单一的类，该类负责加入独立的或不兼容的接口功能。  
适配器继承或依赖已有的对象，实现想要的目标接口。  
**优点：**  
>1. 可以让任何两个没有关联的类一起运行。  
>2. 提高了类的复用。  
>3. 增加了类的透明度。  
>4. 灵活性好。  
>
>**缺点：**
>1. 过多使用适配器会让系统凌乱，不易整体把握，没有必要，不建议使用适配器模式  
>
>**注意事项：**   
>适配器不是在详细设计时使用的，而是在解决正在服役的项目使用的

```
<?php
/**
 * adapter pattern  适配器模式
 */
// 对象适配器
// 定义接口
interface target
{
	public function echoSample1();
	public function echoSample2();
}
class adapterOne implements target
{
	public function echoSample1()
	{
		echo "+++++";
	}
	public function echoSample2()
	{
		
	}
}
class adapterTwo implements target
{
	private $adapterOne;
	public function __construct(adapterOne $obj)
	{
		$this->adapterOne = $obj;
	}
	public function echoSample1()
	{
		$this->adapterOne->echoSample1();
	}
	public function echoSample2()
	{
		echo '----';
	}
}
$adapterTwo = new adapterTwo(new adapterOne);
$adapterTwo->echoSample1();
$adapterTwo->echoSample2();

// 类适配器
interface target2
{
	public function echoSample1();
	public function echoSample2();
}
class adapterClassOne
{
	public function echoSample1()
	{
		echo "****";
	}
}
class adapterClassTwo extends adapterClassOne implements target2
{
	public function echoSample2()
	{
		echo "&&&&";
	}
}
$adapterClassTwo = new adapterClassTwo();
$adapterClassTwo->echoSample1();
$adapterClassTwo->echoSample2();
```

### 桥接模式
>把抽象化与实现化解耦，使得二者可以独立变化  
抽象类依赖实现类。  
**优点：**  
>1. 抽象和实现的分离。 
>2. 优秀的扩展能力。 
>3. 实现细节对客户透明。
>
>**缺点：**  
桥接模式的引入会增加系统的理解与设计难度，由于聚合关联关系建立在抽象层，要求开发者针对抽象进行设计与编程。  
>**注意事项：**  
对于两个独立变化的维度，使用桥接模式再适合不过了。

```
<?php
/**
 * 桥接模式 -- bridge pattern
 */
// 定义接口--对象
interface DrawApi
{
	public function draw();
}
class RedColor implements DrawApi
{
	public function draw()
	{
		echo "red color";
	}
}
class GreenColor implements DrawApi
{
	public function draw()
	{
		echo "green color";
	}
}
// 定义抽象类
abstract class Shape
{
	abstract public function draw();
}
class Circle extends Shape
{
	private $drawApi;
	public function __construct(DrawApi $obj)
	{
		$this->drawApi = $obj;
	}
	public function draw()
	{
		$this->drawApi->draw();
	}
}

$obj = new Circle(new RedColor());
$obj->draw();
$obj2 = new Circle(new GreenColor());
$obj2->draw();
```

### 过滤器模式  
>使用不同的标准来过滤一组对象，通过逻辑运算以解耦的方式把它们连接起来

```
<?php
/**
 * 过滤器模式 -- filter pattern
 */
// 定义接口
interface Shape
{
	public function arrayPrint(array $arr);
}
class One implements Shape
{
	public function arrayPrint(array $arr)
	{
		$array = array();
		foreach($arr as $v){
			if($v->getName() == 'one'){
				$array[] = $v;
			}
		}
		return $array;
	}
}
class Two implements Shape
{
	public function arrayPrint(array $arr)
	{
		$array = array();
		foreach($arr as $v){
			if($v->getName() == 'two'){
				$array[] = $v;
			}
		}
		return $array;
	}
}
class Three implements Shape
{
	public function arrayPrint(array $arr)
	{
		$array = array();
		foreach($arr as $v){
			if($v->getID() == '2'){
				$array[] = $v;
			}
		}
		return $array;
	}
}

// 定义标准类
class Data
{
	private $id;
	private $name;
	public function __construct($id, $name)
	{
		$this->id = $id;
		$this->name = $name;
	}
	public function getID()
	{
		return $this->id;
	}
	public function getName()
	{
		return $this->name;
	}
}

$array = array();
$array[] = new Data('1', 'one');
$array[] = new Data('2', 'one');
$array[] = new Data('3', 'two');
$array[] = new Data('4', 'three');

$one = new One();
arrayPrint($one->arrayPrint($array));
$two = new Two();
arrayPrint($two->arrayPrint($array));

$three = new Three();
arrayPrint($three->arrayPrint($one->arrayPrint($array)));

function arrayPrint($arr)
{
	foreach($arr as $v){
		echo $v->getID();
		echo $v->getName();
	}
}
```

### 组合模式
>一组相似的对象当作一个单一的对象  
组合模式依据树形结构来组合对象，用来表示部分以及整体层次  
**意图：**  
将对象组合成树形结构以表示"部分-整体"的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。  
**优点：**
>1. 高层模块调用简单。
>2. 节点自由增加。
>
>**缺点：**  
在使用组合模式时，其叶子和树枝的声明都是实现类，而不是接口，违反了依赖倒置原则。  
**注意事项：**  
定义时为具体类。

```
<?php
/**
 * 组合模式  -- composite pattern
 */
interface Shape
{
	public function add($obj);
	public function remove($obj);
	public function operator();
}
class Composite implements Shape
{
	private $_composite;
	public function __construct()
	{
		$this->_composite = array();
	}
	public function operator()
	{
		foreach($this->_composite as $v){
			$v->operator();
		}
	}
	public function add($obj)
	{
		$this->_composite[] = $obj;
	}
	public function remove($obj)
	{
		foreach($this->_composite as $k=>$v){
			if($obj == $v){
				unset($this->_composite[$k]);
				return true;
			}
		}
		return false;
	}
}

class Leaf implements Shape
{
	private $_name;
	public function __construct($name)
	{
		$this->_name = $name;
	}
	public function add($obj){}
	public function remove($obj){}
	public function operator()
	{
		echo $this->_name;
	}
}
$leaf1 = new Leaf('one');
$leaf2 = new Leaf('two');
$composite = new Composite();
$composite->add($leaf1);
$composite->add($leaf2);
$composite->operator();
$composite->remove($leaf1);
$composite->operator();
```

### 装饰器模式
>允许向一个现有的对象添加新的功能，同时又不改变其结构  
**优点：**
装饰类和被装饰类可以独立发展，不会相互耦合，装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能。  
**缺点：**  
多层装饰比较复杂。  
**注意事项：**  
可代替继承。

```
<?php
/**
 * 装饰器模式  -- decorator pattern
 */
// 定义接口
interface Shape
{
	public function draw();
}
class Decorator implements Shape
{
	private $_decorator;
	public function __construct(Shape $decorator)
	{
		$this->_decorator = $decorator;
	}
	public function draw()
	{
		$this->_decorator->draw();
	}
}
class RedColor extends Decorator
{
	public function __construct(Shape $decorator)
	{
		parent::__construct($decorator);
	}
	public function draw()
	{
		parent::draw();
		$this->echoDraw();
	}
	public function echoDraw()
	{
		echo "red color";
	}
}
class GreenColor extends Decorator
{
	public function __construct(Shape $decorator)
	{
		parent::__construct($decorator);
	}
	public function draw()
	{
		parent::draw();
		$this->echoDraw();
	}
	public function echoDraw()
	{
		echo "green color";
	}
}
class Color implements Shape
{
	public function draw(){
		echo "color";
	}
}
$obj = new Color();
$obj_red = new RedColor($obj);
$obj_green = new GreenColor($obj_red);
$obj_red->draw();
$obj_green->draw();
```

### 外观模式
>隐藏系统的复杂性，并向客户端提供了一个客户端可以访问系统的接口  
在客户端和复杂系统之间再加一层，这一层将调用顺序、依赖关系等处理好。  
**优点：**
>1. 减少系统相互依赖。 
>2. 提高灵活性。 
>3. 提高了安全性。
>
>**缺点：**  
不符合开闭原则，如果要改东西很麻烦，继承重写都不合适。  
**注意事项：**  
在层次化结构中，可以使用外观模式定义系统中每一层的入口。

```
<?php
/**
 * 门面模式/外观模式  -- facade pattern
 */
//定义接口
interface Shape
{
	public function draw();
}
class Red implements Shape
{
	public function draw()
	{
		echo "red";
	}
}
class Green implements Shape
{
	public function draw()
	{
		echo "green";
	}
}
class Demo
{
	private $_red;
	private $_green;
	public function __construct()
	{
		$this->_red = new Red();
		$this->_green = new Green();
	}
	public function draw()
	{
		$this->_red->draw();
		$this->_green->draw();
	}
}
$obj = new Demo();
$obj->draw();
```

### 享元模式
>减少创建对象的数量，以减少内存占用和提高性能  
用 HashMap 存储这些对象。  
**优点：**  
大大减少对象的创建，降低系统的内存，使效率提高。  
**缺点：**  
提高了系统的复杂度，需要分离出外部状态和内部状态，而且外部状态具有固有化的性质，不应该随着内部状态的变化而变化，否则会造成系统的混乱。  
**注意事项：**  
>1. 注意划分外部状态和内部状态，否则可能会引起线程安全问题。 
>2. 这些类必须有一个工厂对象加以控制

```
<?php
/**
 * 享元模式  -- flyweight pattern
 */
abstract class Resource
{
	private $_resource = null;
	abstract public function operator();
}
class UnShare extends Resource
{
	public function __construct($str)
	{
		$this->_resource = $str;
	}
	public function operator()
	{
		echo $this->_resource;
	}
}
class Share extends Resource
{
	private $_resources = array();
	public function setResource($str)
	{
		if(isset($this->_resources[$str])){
			return $this->_resources[$str];
		}else{
			return $this->_resources[$str] = $str;
		}
	}
	public function operator()
	{
		foreach($this->_resources as $key=>$val){
			echo $key . '=>' . $val;
		}
	}
}
$obj = new Share();
$obj->setResource('a');
$obj->operator();
$obj->setResource('b');
$obj->operator();
$objUnShare = new UnShare('A');
$objUnShare->operator();
$objUnShare = new UnShare('B');
$objUnShare->operator();
```

### 代理模式
>为其他对象提供一种代理以控制对这个对象的访问。  
实现与被代理类组合。  
**优点：**
>1. 职责清晰。 
>2. 高扩展性。 
>3. 智能化。  
>
>**缺点：** 
>1. 由于在客户端和真实主题之间增加了代理对象，因此有些类型的代理模式可能会造成请求的处理速度变慢。
>2. 实现代理模式需要额外的工作，有些代理模式的实现非常复杂。
>
>**注意事项：** 
>1. 和适配器模式的区别：适配器模式主要改变所考虑对象的接口，而代理模式不能改变所代理类的接口。 
>2. 和装饰器模式的区别：装饰器模式为了增强功能，而代理模式是为了加以控制。

```
<?php
/**
 * 代理模式  -- proxy pattern
 */
abstract class Subject
{
	abstract public function draw();
}
class RealSubject extends Subject
{
	public function draw()
	{
		echo "real subject";
	}
}
class ProxySubject extends Subject
{
	private $_subject = null;
	public function draw()
	{
		$this->before();
		if(is_null($this->_subject)){
			$this->_subject = new RealSubject();
		}
		$this->_subject->draw();
		$this->after();
	}
	public function before()
	{
		echo "before";
	}
	public function after()
	{
		echo "after";
	}
}
$obj = new ProxySubject();
$obj->draw();
```