---
title: PHP设计模式之创建型模式（creational patterns）
tags: 设计模式
categories: 设计模式
---
### 工厂模式
>定义一个创建对象的接口，让其子类决定去实例化哪一个工厂类  
创建的过程是在子类中执行  
**优点**：  
>1. 调用者想创建一个对象只需要知道名称即可  
>2. 扩展性高，想增加一个产品，只需要扩展一个工厂类就可以  
>3. 屏蔽产品具体实现，调用者只关心产品的接口  
>
>**缺点**：  
>1. 每增加一个产品，都需要增加一个具体类和对象实现工厂，使得系统类的个数成倍增加，一定程度上增加了系统复杂度，同时也增加了系统具体类的依赖

```
/*** ------------工厂模式  factory-----------------------  ***/

class DB
{
    public function __construct()
    {
        echo get_class();
    }
    public function die()
    {

    }
}
class Mysql extends DB{}
class SqlSrv extends DB{}
class Odbc extends DB{}


interface TestFactory
{
    public function toString();
}

class Factory implements TestFactory
{
    public function toString(){

    }
    public function do($type)
    {
        switch($type){
            case 'Mysql':
                return new Mysql();
            case 'SqlSrv':
                return new SqlSrv();
            case 'Odbc':
                return new Odbc();
        }
    }
}

$test = new Factory();
$test->do('Mysql');
$test->do('SqlSrv');
$test->do('Odbc');
```

### 抽象工厂模式
>围绕一个超级工厂创建其他工厂，该超级工厂是其他工厂的工厂  
一个工厂中聚合多个同类产品  
**优点**：  
    当一个产品族中的多个对象被设计成一起工作时，它能保证客户端始终只使用同一个产品族中的对象。  
**缺点**：  
    产品族扩展非常困难，要增加一个系列的某一产品，既要在抽象的 Creator 里加代码，又要在具体的里面加代码。  
**注意事项**：  
    产品族难扩展，产品等级易扩展。

```
/*** ------------抽象工厂模式  abstract factory-----------------------  ***/
class Cache{}
class CacheDie{}
class FileCache extends Cache
{
    public function __construct()
    {
        echo "filecache";
    }
}
class RedisCache extends Cache{
    public function __construct()
    {
        echo "rediscache";
    }
}
class FileCacheDie extends CacheDie
{
    public function __construct()
    {
        echo "FileCacheDie";
    }
}
class RedisCacheDie extends CacheDie
{
    public function __construct()
    {
        echo "RedisCacheDie";
    }
}

class CacheAbstractFactory
{
    public function createCache($type){
        switch ($type) {
            case 'file':
                return new FileCache();
            case 'redis':
                return new RedisCache();
            default:
                break;
        }
    }
}
class CacheDieAbstractFactory
{
    public function createCache($type){
        switch ($type) {
            case 'file':
                return new FileCacheDie();
            case 'redis':
                return new RedisCacheDie();
            default:
                break;
        }
    }
}

class TestAbstractFactory
{
    public function createType($type)
    {
        switch ($type) {
            case 'cache':
                return new CacheAbstractFactory();
            case 'cacheDie':
                return new CacheDieAbstractFactory();
            default:
                break;
        }
    }
}

$testAbstractFactory = new TestAbstractFactory();
$cache = $testAbstractFactory->createType('cache');
$cache->createCache('file');
$cache->createCache('redis');
$cacheDie = $testAbstractFactory->createType('cacheDie');
$cacheDie->createCache('file');
$cacheDie->createCache('redis');

/*** -----------------------------------  ***/
```

### 单例模式
>负责创建自己的对象，同时确保只有单个对象被创建。  
这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。  
**注意**：  
>1. 单例类只能有一个实例。  
>2. 单例类必须自己创建自己的唯一实例。  
>3. 单例类必须给所有其他对象提供这一实例。  
>
>**优点**：  
>1. 在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例  
>2. 避免对资源的多重占用（比如写文件操作）。  
>
>**缺点**：  
    没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。  
**注意事项**：  
    getInstance() 方法中需要使用同步锁 synchronized (Singleton.class) 防止多线程同时进入造成 instance 被多次实例化。

```
<?php

/*** -----------单例模式  singleton------------  ***/
final class TestSingle
{

    private static $instance;
    public static function getInstance()
    {
        if(!(self::$instance instanceof self )){
            self::$instance = new self();
        }
        return self::$instance;
    }

    private function __construct()
    {

    }

    public function toString()
    {
        echo "string";
    }
}


$test = TestSingle::getInstance();
$test->toString();

/*** -----------------------------------  ***/
```

### 建造者模式
>一个 Builder 类会一步一步构造最终的对象。该 Builder 类是独立于其他对象的。  
**优点**：  
>1. 建造者独立，易扩展。  
>2. 便于控制细节风险。  
>
>**缺点**：  
>1. 产品必须有共同点，范围有限制。  
>2. 如内部变化复杂，会有很多的建造类。  
>
>**注意事项**：  
    与工厂模式的区别是：建造者模式更加关注与零件装配的顺序。

```
/*** --------------建造者模式  builder---------------------  ***/

class Product
{
    private $_arr;
    public function __construct()
    {
        $this->_arr = array();
    }
    public function add($part)
    {
        return array_push($this->_arr, $part);
    }
}

class Builder
{
    private $_product;
    public function __construct()
    {
        $this->_product = new Product();
    }
    public function add1($part)
    {
        $this->_product->add($part);
    }
    public function add2($part)
    {
        $this->_product->add($part);
    }
    public function getPart()
    {
        return $this->_product;
    }
}

class Director
{
    public function __construct(Builder $builder)
    {
        $builder->add1('arr1');
        $builder->add2('add2');
    }
}

$test_builder = new Builder();
$test_director = new Director($test_builder);
print_r($test_builder->getPart());

/*** -----------------------------------  ***/
```

### 原型模式
>实现了一个原型接口，该接口用于创建当前对象的克隆。  
实现克隆 clone  
**优点**：  
>1. 性能提高。  
>2. 逃避构造函数的约束。  
>
>**缺点**：  
>1. 配备克隆方法需要对类的功能进行通盘考虑，这对于全新的类不是很难，但对于已有的类不一定很容易，特别当一个类引用不支持串行化的间接对象，或者引用含有循环结构的时候。  
>2. 必须实现 Cloneable 接口。  
>
>**注意事项**：  
    与通过对一个类进行实例化来构造新对象不同的是，原型模式是通过拷贝一个现有对象生成新对象的。浅拷贝实现 Cloneable，重写，深拷贝是通过实现 Serializable 读取二进制流。
    
```
/*** --------------原型模式  prototype---------------------  ***/

class ProtoType
{
    private $_name;
    public function __construct($obj)
    {
        $this->_name = $obj;
    }
    public function copy()
    {
        return clone $this;
    }
}
class TestProto{}
$testProto = new ProtoType(new TestProto());
print_r($testProto);
$testProtoCopy = $testProto->copy();
print_r($testProtoCopy);

/*** -----------------------------------  ***/
```