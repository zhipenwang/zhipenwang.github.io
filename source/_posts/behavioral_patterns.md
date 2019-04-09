---
title: PHP设计模式之行为型模式（behavioral patterns）
date: 2018-07-01 20:21:01
tags: 设计模式
categories: 设计模式
---
### 责任链模式
>为请求创建了一个接收者对象的链  
这种模式给予请求的类型，对请求的发送者和接收者进行解耦  
**优点：**  
>1. 降低耦合度。它将请求的发送者和接收者解耦。 
>2. 简化了对象。使得对象不需要知道链的结构。 
>3. 增强给对象指派职责的灵活性。通过改变链内的成员或者调动它们的次序，允许动态地新增或者删除责任。 
>4. 增加新的请求处理类很方便。
>
>**缺点：**
>1. 不能保证请求一定被接收。 
>2. 系统性能将受到一定影响，而且在进行代码调试时不太方便，可能会造成循环调用。 
>3. 可能不容易观察运行时的特征，有碍于除错

```
<?php
/**
 * 责任链模式  -- responsibility pattern
 */
abstract class Responsibility
{
	protected $next;
	public function setNext(Responsibility $obj)
	{
		$this->next = $obj;
		return $this;
	}
	abstract public function operator();
}
class ResponsibilityA extends Responsibility
{
	public function operator()
	{
		if(false == is_null($this->next)){
			$this->next->operator();
			echo "this is responsibility A";
		}
	}
}
class ResponsibilityB extends Responsibility
{
	public function operator()
	{
		if(false == is_null($this->next)){
			$this->next->operator();
			echo "this is responsibility B";
		}
	}
}
$responsibilityA = new ResponsibilityA();
$responsibilityB = new ResponsibilityB();
$responsibilityA->setNext($responsibilityB);
$responsibilityA->operator();
```

### 命令模式
>请求以命令的形式包裹在对象中，并传给调用对象。  
调用对象寻找可以处理该命令的合适的对象，并把该命令传给相应的对象，该对象执行命令。  
通过调用者调用接受者执行命令，顺序：调用者→接受者→命令。  
**优点：**
>1. 降低了系统耦合度。
>2. 新的命令可以很容易添加到系统中去。
>
>**缺点：**  
使用命令模式可能会导致某些系统有过多的具体命令类。

```
<?php
/**
 * 命令模式  -- commandpattern
 */
// 定义接口
interface Command
{
	public function execute();
}
class RealCommand implements Command
{
	private $_receiver;
	public function __construct(Receiver $receiver)
	{
		$this->_receiver = $receiver;
	}
	public function execute()
	{
		$this->_receiver->action();
	}
}
// 接受者
class Receiver
{
	private $_name;
	public function __construct($name)
	{
		$this->_name = $name;
	}
	public function action()
	{
		echo "this is receiver" . $this->_name;
	}
}
// 请求者
class Invoker
{
	private $_realCommand;
	public function __construct(RealCommand $realCommand)
	{
		$this->_realCommand = $realCommand;
	}
	public function operator()
	{
		$this->_realCommand->execute();
	}
}
$receiver = new Receiver('hello world');
$realCommand = new RealCommand($receiver);
$invoker = new Invoker($realCommand);
$invoker->operator();
```

### 解释器模式
>一个表达式接口，该接口解释一个特定的上下文。这种模式被用在 SQL 解析、符号处理引擎等。  
**优点：**
>1. 可扩展性比较好，灵活。 
>2. 增加了新的解释表达式的方式。 
>3. 易于实现简单文法。
>
>**缺点：**
>1. 可利用场景比较少。 
>2. 对于复杂的文法比较难维护。 
>3. 解释器模式会引起类膨胀。 
>4. 解释器模式采用递归调用方法。

```
<?php
/**
 * 解释器模式  -- interpreter pattern
 */
interface Expression
{
	public function interpreter($data);
}
class NumberExpression implements Expression
{
	public function interpreter($num)
	{
		switch($num){
			case '0': return '零';
			case '1': return '一';
			case '2': return '二';
			case '3': return '三';
			case '4': return '四';
			case '5': return '五';
			case '6': return '六';
			case '7': return '七';
			case '8': return '八';
			case '9': return '久';
			default: return '无';
		}
	}
}
class StringExpression implements Expression
{
	public function interpreter($str)
	{
		return strtoupper($str);
	}
}
class Interpreter
{
	public function execute($string)
	{
		$expression = null;
		for($i=0; $i<strlen($string); $i++){
			if(is_numeric($string[$i])){
				$expression = new NumberExpression();
			}elseif(is_string($string[$i])){
				$expression = new StringExpression();
			}
			echo $expression->interpreter($string[$i]);
			echo "<br/>";
		}
	}
}
$obj = new Interpreter();
$obj->execute('123sqwe09843');
```

### 迭代器模式
>顺序访问集合对象的元素，不需要知道集合对象的底层表示。  
**优点：**
>1. 它支持以不同的方式遍历一个聚合对象。 
>2. 迭代器简化了聚合类。 
>3. 在同一个聚合上可以有多个遍历。 
>4. 在迭代器模式中，增加新的聚合类和迭代器类都很方便，无须修改原有代码。
>
>**缺点：**  
由于迭代器模式将存储数据和遍历数据的职责分离，增加新的聚合类需要对应增加新的迭代器类，类的个数成对增加，这在一定程度上增加了系统的复杂性。
>
>**注意事项：**  
迭代器模式就是分离了集合对象的遍历行为，抽象出一个迭代器类来负责，这样既可以做到不暴露集合的内部结构，又可让外部代码透明地访问集合内部的数据。

```
<?php
/**
 * 迭代器模式  --  iterator pattern
 */
class Sample implements Iterator
{
	private $_item;
	public function __construct($data)
	{
		$this->_item = $data;
	}
	public function current()
	{
		return current($this->_item);
	}
	public function next()
	{
		return next($this->_item);
	}
	public function key()
	{
		return key($this->_item);
	}
	public function rewind()
	{
		reset($this->_item);
	}
	public function valid()
	{
		return ($this->current() !== false);
	}
}
$data = array('1', '2', '3', '4', '5');
$obj = new Sample($data);
echo $obj->current();
echo $obj->next();
echo $obj->key();
echo $obj->rewind();
echo $obj->valid();
```

### 中介者模式
>降低多个对象和类之间的通信复杂性。  
该类通常处理不同类之间的通信，并支持松耦合，使代码易于维护。  
**优点：**
>1. 降低了类的复杂度，将一对多转化成了一对一。 
>2. 各个类之间的解耦。 3、符合迪米特原则。
>
>**缺点：**  
中介者会庞大，变得复杂难以维护。

```
<?php
/**
 * 中介者模式  -- mediator pattern
 */
abstract class Mediator
{
	abstract public function send($message, $colleague);
}
abstract class Colleague
{
	private $_mediator;
	public function __construct($mediator)
	{
		$this->_mediator = $mediator;
	}
	public function send($message)
	{
		$this->_mediator->send($message, $this);
	}
	abstract public function notify($message);
}
class Colleague1 extends Colleague
{
	public function notify($message)
	{
		echo "colleague1 " . $message;
	}
}
class Colleague2 extends Colleague
{
	public function notify($message)
	{
		echo "colleague2 " . $message;
	}
}
class NewMediator extends Mediator
{
	private $_colleauge1;
	private $_colleauge2;
	public function set($col1, $col2)
	{
		$this->_colleauge1 = $col1;
		$this->_colleauge2 = $col2;
	}
	public function send($message, $colleague)
	{
		if($this->_colleauge1 == $colleague){
			$this->_colleauge1->notify($message);
		}else{
			$this->_colleauge2->notify($message);
		}
	}
}
$newMediator = new NewMediator();
$col1 = new Colleague1($newMediator);
$col2 = new Colleague2($newMediator);
$newMediator->set($col1, $col2);
$col1->send('hello col1');
$col2->send('hello col2');
```

### 备忘录模式
>保存一个对象的某个状态，以便在适当的时候恢复对象  
所谓备忘录模式就是在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态  
通过一个备忘录类专门存储对象状态  
客户不与备忘录类耦合，与备忘录管理类耦合  
>**优点：**  
>1. 给用户提供了一种可以恢复状态的机制，可以使用户能够比较方便地回到某个历史的状态。 
>2. 实现了信息的封装，使得用户不需要关心状态的保存细节。  
>
>**缺点：**  
消耗资源。如果类的成员变量过多，势必会占用比较大的资源，而且每一次保存都会消耗一定的内存。  
>
>**注意事项：**  
>1. 为了符合迪米特原则，还要增加一个管理备忘录的类。 
>2. 为了节约内存，可使用原型模式+备忘录模式。

```
<?php
/**
 * 备忘录模式  --  memento pattern
 */
class Memento
{
	private $_memento;
	public function __construct(string $state)
	{
		$this->_memento = $state;
	}
	public function getState()
	{
		return $this->_memento;
	}
}
class Originator
{
	private $_state;
	public function setState(string $state)
	{
		$this->_state = $state;
	}
	public function getState()
	{
		return $this->_state;
	}
	public function saveMemento()
	{
		return new Memento($this->_state);
	}
	public function getMemento(Memento $memento)
	{
		$this->_state = $memento->getState();
	}
}
class CakeTask
{
	private $_mementoList;
	public function __construct()
	{
		$this->_mementoList = array();
	}
	public function add(Memento $memento)
	{
		$this->_mementoList[] = $memento;
	}
	public function get(int $index)
	{
		return $this->_mementoList[$index];
	}
}
$originator = new Originator();
$cakeTask = new CakeTask();
$originator->setState('hello one');
$originator->setState('hello two');
$cakeTask->add($originator->saveMemento());
$originator->setState('hello three');
$cakeTask->add($originator->saveMemento());
$originator->setState('hello four');

echo $originator->getState();
$originator->getMemento($cakeTask->get(0));
echo $originator->getState();
$originator->getMemento($cakeTask->get(1));
echo $originator->getState();
```

### 观察者模式
>对象间存在一对多关系  
当一个对象被修改时，则会自动通知它的依赖对象并被自动更新  
**优点：**
>1. 观察者和被观察者是抽象耦合的。 
>2. 建立一套触发机制。
>
>**缺点：**
>1. 如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。 
>2. 如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。 
>3. 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。
>
>**注意事项：**
>1. 避免循环引用。 
>2. 如果顺序执行，某一观察者错误会导致系统卡壳，一般采用异步方式。

```
<?php
/**
 * 观察者模式  -- observer pattern
 */
// 定义接口
interface Observer
{
	public function onSendMsg($name);
	public function getName();
}
class UserListLogger implements Observer
{
	public function onSendMsg($name)
	{
		echo $name . ' send to UserListLogger';
	}
	public function getName()
	{
		return 'userlist_logger';
	}
}
class OtherObserver implements Observer
{
	public function onSendMsg($name)
	{
		echo $name . ' send to OtherObserver';
	}
	public function getName()
	{
		return 'other_observer';
	}
}
interface Observerable
{
	public function add(Observer $observer);
	public function remove($name);
}
class UserList implements Observerable
{
	private $_observer = array();
	public function add(Observer $observer)
	{
		$this->_observer[] = $observer;
	}
	public function remove($name)
	{
		foreach($this->_observer as $k=>$val){
			if($val->getName() == $name){
				unset($this->_observer[$k]);
			}
		}
	}
	public function sendMsg($name)
	{
		foreach($this->_observer as $val){
			$val->onSendMsg($name);
		}
	}
}
$userList = new UserList();
$userList->add(new UserListLogger());
$userList->add(new OtherObserver());
$userList->sendMsg('jone');
$userList->remove('userlist_logger');
$userList->sendMsg('jami');
```

### 状态模式
>类的行为是基于它的状态改变的  
对象的行为依赖于它的状态（属性），并且可以根据它的状态改变而改变它的相关行为。  
**优点：**
>1. 封装了转换规则。 
>2. 枚举可能的状态，在枚举状态之前需要确定状态种类。 
>3. 将所有与某个状态有关的行为放到一个类中，并且可以方便地增加新的状态，只需要改变对象状态即可改变对象的行为。 
>4. 允许状态转换逻辑与状态对象合成一体，而不是某一个巨大的条件语句块。 
>5. 可以让多个环境对象共享一个状态对象，从而减少系统中对象的个数。
>
>**缺点：**
>1. 状态模式的使用必然会增加系统类和对象的个数。 
>2. 状态模式的结构与实现都较为复杂，如果使用不当将导致程序结构和代码的混乱。 
>3. 状态模式对"开闭原则"的支持并不太好，对于可以切换状态的状态模式，增加新的状态类需要修改那些负责状态转换的源代码，否则无法切换到新增状态，而且修改某个状态类的行为也需修改对应类的源代码。
>
>**注意事项：**  
在行为受状态约束的时候使用状态模式，而且状态不超过 5 个。

```
<?php
/**
 * 状态模式  --  state pattern
 */
interface State
{
	public function handle(Context $context);
}
class StateA implements State
{
	private static $_instance;
	private function __construct()
	{

	}
	public static function getInstance()
	{
		if(is_null(self::$_instance)){
			self::$_instance = new StateA();
		}
		return self::$_instance;
	}
	public function handle(Context $context)
	{
		echo "context A";
		$context->setState(StateB::getInstance());
	}
}
class StateB implements State
{
	private static $_instance;
	private function __construct()
	{

	}
	public static function getInstance()
	{
		if(is_null(self::$_instance)){
			self::$_instance = new StateB();
		}
		return self::$_instance;
	}
	public function handle(Context $context)
	{
		echo "context B";
		$context->setState(StateA::getInstance());
	}
}
class Context{
	private $_state;
	public function __construct()
	{
		$this->_state = StateA::getInstance();
	}
	public function setState(State $state)
	{
		$this->_state = $state;
	}
	public function request()
	{
		$this->_state->handle($this);
	}
}
$context = new Context();
$context->request();
$context->request();
$context->request();
$context->request();
```

### 空对象模式
>一个空对象取代 NULL 对象实例的检查  
在空对象模式中，我们创建一个指定各种要执行的操作的抽象类和扩展该类的实体类，还创建一个未对该类做任何实现的空对象类，该空对象类将无缝地使用在需要检查空值的地方。

```
<?php
/**
 * 空对象模式  --  null object pattern
 */
abstract class AbstractCustom
{
	private $_name;
	abstract public function isNil();
	abstract public function getName();
}
class RealCustom extends AbstractCustom
{
	private $_name;
	public function __construct($name)
	{
		$this->_name = $name;
	}
	public function getName()
	{
		return $this->_name;
	}
	public function isNil()
	{
		return false;
	}
}
class NullCustom extends AbstractCustom
{
	public function getName()
	{
		return "this is null custom";
	}
	public function isNil()
	{
		return false;
	}
}
class NullFactory
{
	private $_arr = array('one', 'two');
	public function getCustom($name){
		if(in_array($name, $this->_arr)){
			return new RealCustom($name);
		}
		return new NullCustom();
	}
}
$obj = new NullFactory();
$test1 = $obj->getCustom('one');
$test2 = $obj->getCustom('one_one');
$test3 = $obj->getCustom('two');
$test4 = $obj->getCustom('two_two');
echo $test1->getName();
echo $test2->getName();
echo $test3->getName();
echo $test4->getName();
```

### 策略模式
>一个类的行为或其算法可以在运行时更改  
定义一系列的算法,把它们一个个封装起来, 并且使它们可相互替换  
**优点：**
>1. 算法可以自由切换。 
>2. 避免使用多重条件判断。 
>3. 扩展性良好。
>
>**缺点：**
>1. 策略类会增多。 
>2. 所有策略类都需要对外暴露。
>
>**注意事项：**  
如果一个系统的策略多于四个，就需要考虑使用混合模式，解决策略类膨胀的问题

```
<?php
/**
 * 策略模式  -- strategy pattern
 */
interface Strategy
{
	public function do_method();
}
class StrategyA implements Strategy
{
	public function do_method()
	{
		echo "this is strategy A";
	}
}
class StrategyB implements Strategy
{
	public function do_method()
	{
		echo "this is strategy B";
	}
}
class StrategyC implements Strategy
{
	public function do_method()
	{
		echo "this is strategy C";
	}
}

class Question
{
	private $_strategy;
	public function __construct($strategy)
	{
		$this->_strategy = $strategy;
	}
	public function handle()
	{
		$this->_strategy->do_method();
	}
}
$a = new Question(new StrategyA());
$a->handle();
$b = new Question(new StrategyB());
$b->handle();
$c = new Question(new StrategyC());
$c->handle();
```

### 模板模式
>一些方法通用，却在每一个子类都重新写了这一方法  
将这些通用算法抽象出来  
在抽象类实现，其他步骤在子类实现  
**优点：**
>1. 封装不变部分，扩展可变部分。 
>2. 提取公共代码，便于维护。 
>3. 行为由父类控制，子类实现。
>
>**缺点：**  
每一个不同的实现都需要一个子类来实现，导致类的个数增加，使得系统更加庞大。
>
>**注意事项：**  
为防止恶意操作，一般模板方法都加上 final 关键词。

```
<?php
/**
 * 模板模式 -- template pattern
 */
abstract class Game
{
	abstract public function start();
	abstract public function end();
	public function action()
	{
		$this->start();
		$this->end();
	}
}
class SuperMary extends Game
{
	public function start()
	{
		echo "the game is starting";
	}
	public function end()
	{
		echo "the game is ending";
	}
}
$superMary = new SuperMary();
$superMary->action();
```

### 访问者模式
>使用了一个访问者类，它改变了元素类的执行算法  
元素的执行算法可以随着访问者改变而改变  
稳定的数据结构和易变的操作耦合问题。  
在数据基础类里面有一个方法接受访问者，将自身引用传入访问者。  
**优点：**
>1. 符合单一职责原则。 
>2. 优秀的扩展性。 
>3. 灵活性。
>
>**缺点：**
>1. 具体元素对访问者公布细节，违反了迪米特原则。 
>2. 具体元素变更比较困难。 
>3. 违反了依赖倒置原则，依赖了具体类，没有依赖抽象。
>
>**注意事项：**  
访问者可以对功能进行统一，可以做报表、UI、拦截器与过滤器。

```
<?php
/**
 *
 */
interface ComputerPart
{
	public function accept(Visitor $visitor);
}
class Computer implements ComputerPart
{
	private $_computerPart = array();
	public function accept(Visitor $visitor)
	{
		foreach($this->_computerPart as $val){
			$val->accept($visitor);
		}
		$visitor->visit($this);
	}
	public function getName()
	{
		return 'computer';
	}
	public function setAttach(ComputerPart $computerPart)
	{
		array_push($this->_computerPart, $computerPart);
	}
}
class Mouse implements ComputerPart
{
	public function accept(Visitor $visitor)
	{
		$visitor->visit($this);
	}
	public function getName()
	{
		return 'mouse';
	}
}
class KeyBoard implements ComputerPart
{
	public function accept(Visitor $visitor)
	{
		$visitor->visit($this);
	}
	public function getName()
	{
		return 'keyboard';
	}
}
class Visitor
{
	public function visit(ComputerPart $computerPart)
	{
		echo "this is ".$computerPart->getName();
	}
}
$computer = new Computer();
$computer->setAttach(new Mouse());
$computer->setAttach(new KeyBoard());
$computer->accept(new Visitor());
```