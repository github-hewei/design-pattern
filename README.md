# 设计模式笔记

## 设计模式简介

设计模式（Design pattern）代表了最佳的实践，通常被有经验的面向对象的软件开发人员所采用。设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。

设计模式是一套被反复使用的，多数人知晓的，经过分类编目的，代码设计经验的总结。使用设计模式是为了重用代码，让代码更容易被他人理解。保证代码的可靠性。毫无疑问，设计模式于己于他人于系统都是多赢的，设计模式使代码编制真正工程化，设计模式是软件工程的基石，如同大厦的一块块砖石一样。项目中合理的运用设计模式可以完美的解决很多问题，每种模式在现实中都有相应的原理来与之对应，每种模式都描述了一个在我们周围不断重复发生的问题，以及该问题的核心解决方案，这也是设计模式能被广泛应用的原因。



## 创建型模式（Creational Patterns）

> 这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是直接使用new运算符直接实例化对象。这使得程序在判断针对给定某个实例需要创建哪些对象时更加灵活

### 工厂模式（Factory Pattern）

```php
<?php
class Banana {

}
```

```php
<?php
class Pear {

}
```

```php
<?php
class Apple {

}
```

```php
<?php
class FruitFactory {
    public static function create($type) {
        $instance = null;
        switch($type) {
            case 'banana':
                $instance = new Banana();
                break;
            case 'pear':
                $instance = new Pear();
                break;
            case 'apple':
                $instance = new Apple();
                break;
        }
        return $instance;
    }
}

$instance = FruitFactory::create('apple');
```

### 抽象工厂模式（Abstract Factory Pattern）

```php
<?php
// 形状接口
interface Shape {
    public function draw();
}
```

```php
<?php
// 圆形抽象类
abstract class Circle implements Shape {
    abstract function draw();
}
```

```php
<?php
// 矩形抽象类
abstract class Rectange implements Shape {
    abstract function draw();
}  
```

```php
<?php
// 图形工厂抽象类
abstract class ShapeFactory {
    abstract function getCircle();
    abstract function getRectange();
    
    public static function create($type)
    {
        switch($type) {
            case 'circle':
                return 
        }
    }
}
```

```php
<?php
// 红色图形工厂类
class RedShapeFactory extends ShapeFactory {
    public function getCircle() {
        return new RedCircle();
    }
    public function getRectange() {
        return null;
    }
}
```

```php
<?php
// 红色的圆
class RedCircle extends Circle {
    public function draw() {
        echo "红色的圆";
    }
}
```

```php
<?php
// 蓝色的圆
class BlueCircle extends Circle {
    public function draw() {
        echo "蓝色的圆";
    }
}
```

```php
<?php
$redShapeFactory = new RedShapeFactory();
$circle = $redShaoeFactory->getCircle();
$circle->draw();

// SuperShapeFactory::create('red')->getCircle()->draw();
```

### 单例模式（Singleton Pattern）

```php
<?php
// 简单的单例模式
class Singleton
{
    private static $instance;

    public static function getInstance()
    {
        if (!self::$instance) {
            self::$instance = new self();
        }
        return self::$instance;
    }

    public function showMessage()
    {
        echo "Message\n";
    }

    private function __construct()
    {
    }

    private function __clone()
    {
    }
}
```

```php
<?php
Singleton::getInstance()->showMessage();
```

```php
<?php
/**
 * @link http://www.yiiframework.com/
 * @copyright Copyright (c) 2008 Yii Software LLC
 * @license http://www.yiiframework.com/license/
 */

namespace yii\base;

use Yii;

/**
 * StaticInstanceTrait provides methods to satisfy [[StaticInstanceInterface]] interface.
 *
 * @see StaticInstanceInterface
 *
 * @author Paul Klimov <klimov.paul@gmail.com>
 * @since 2.0.13
 */
trait StaticInstanceTrait
{
    /**
     * @var static[] static instances in format: `[className => object]`
     */
    private static $_instances = [];


    /**
     * Returns static class instance, which can be used to obtain meta information.
     * @param bool $refresh whether to re-create static instance even, if it is already cached.
     * @return static class instance.
     */
    public static function instance($refresh = false)
    {
        $className = get_called_class();
        if ($refresh || !isset(self::$_instances[$className])) {
            self::$_instances[$className] = Yii::createObject($className);
        }
        return self::$_instances[$className];
    }
}
```

### 建造者模式（Builder Pattern）

```php
<?php
// 单个项目接口类
interface Item {
    public function name();

    public function packing();

    public function price();
}
```

```php
<?php
// 包装接口类
interface Packing {
    public function pack();
}
```

```php
<?php
// 纸质包装
class Wrapper implements Packing {
    public function pack()
    {
        return "Wrapper";
    }
}
```

```php
<?php
// 瓶子包装
class Bottle implements Packing {
    public function pack()
    {
        return "Bottle";
    }
}
```

```php
<?php
// 汉堡包抽象类
abstract class Burger implements Item {

    public function packing() {
        return new Wrapper();
    }

    abstract function name();

    abstract function price();
}
```

```php
<?php
// 鸡肉汉堡包类
class ChickenBurger extends Burger {

    public function name()
    {
        return "Chicken Burger";
    }

    public function price()
    {
        return 0.5;
    }
}
```

```php
<?php
// 冷饮抽象类
abstract class ColdDrink implements Item {

    public function packing() {
        return new Bottle();
    }

    abstract function name();

    abstract function price();
}
```

```php
<?php
// 可口可乐类
class Coke extends ColdDrink {

    public function name()
    {
        return "Coke";
    }

    public function price()
    {
        return 2.5;
    }
}
```

### 原型模式（Prototype Pattern）

```php
<?php
// 原型模式抽象类
abstract class Prototype {
    abstract function __clone();
}
```

```php
<?php
// 地图类
class Map extends Prototype {
    public $width;

    public $height;

    public $sea;

    public function setAttributes($attributes) {
        foreach($attributes as $key => $value) {
            $this->$key = $value;
        }
    }

	// 实现 $this->sea 的深拷贝，否则默认为浅拷贝，实例和实例副本的sea属性指向同一个地址。
    public function __clone() {
        $this->sea = clone $this->sea;
    }
}
```

```php
<?php
// 海洋类，加载比较耗时
class Sea {
    public function __construct() {
        sleep(3);
    }
}
```

```php
<?php
// 创建一个 100*100 的地图
$map = new Map();
$map->setAttributes(['width' => 100, 'height' => 100, 'sea' => (new Sea)]);

// clone 上面的地图，再创建一个 200*200 的地图
$map_clone = clone $map;
$map_clone->setAttributes(['width' => 200, 'height' => 200]);

// 优点，不重复实例化 Sea 类，执行效率高
var_dump($map, $map_clone);
exit;

```

## 结构型模式（Structural Patterns）

> 这些设计模式关注类和对象的组合。继承的概念用来组合接口和定义组合对象获得新功能的方式。

### 适配器模式（Adapter Pattern）

```php
<?php
// 媒体播放器接口类
interface MediaPlayer {
    public function play($audioType, $fileName);
}
```

```php
<?php
interface AdvanceMediaPlayer {

    public function playVlc($fileName);

    public function playMp4($fileName);

}
```

```php
<?php
class VlcPlayer implements AdvanceMediaPlayer {

    public function playVlc($fileName) {
        echo "playing vlc file. Name: " . $fileName;
    }

    public function playMp4($fileName) {
        // 什么都不用做
    }

}
```

```php
<?php
class Mp4Player implements AdvanceMediaPlayer {

    public function playVlc($fileName) {
        // 什么都不用做
    }

    public function playMp4($fileName) {
        echo "playing mp4 file. Name: " . $fileName;
    }

}
```

```php
<?php
class MediaAdapter implements MediaPlayer {

    /**
     * @var $advancedMusicPlayer AdvanceMediaPlayer
     */
    protected $advancedMusicPlayer;

    public function __construct($audioType) {
        if ($audioType == 'vlc') {
            $this->advancedMusicPlayer = new VlcPlayer();

        } else if ($audioType == 'mp4') {
            $this->advancedMusicPlayer = new Mp4Player();

        }
    }

    public function play($audioType, $fileName) {
        if ($audioType == 'vlc') {
            $this->advancedMusicPlayer->playVlc($fileName);

        } else if ($audioType == 'mp4') {
            $this->advancedMusicPlayer->playMp4($fileName);

        }
    }

}
```

```php
<?php
class AudioPlayer implements MediaPlayer {

    public function play($audioType, $fileName) {
        if ($audioType == 'mp3') {
            echo "playing mp4 file. Name: " . $fileName;

        } else if ($audioType == 'vlc' || $audioType == 'mp4') {
            $mediaAdapter = new MediaPlayer($audioType);
            $mediaAdapter->play($audioType, $fileName);
        } else {
            echo "Invalid media. " . $audioType . " format not supported";
        }
    }

}
```

```php
<?php
class AdapterPatternDemo {

    $audioPlayer = new AudioPlayer();

    $audioPlayer->play("mp3", "beyond the horizon.mp3");
    $audioPlayer->play("mp4", "alone.mp4");
    $audioPlayer->play("vlc", "far far away.vlc");
    $audioPlayer->play("avi", "mind me.avi");

}
```

### 桥接模式（Bridge Pattern）

```php
<?php
// 绘制类接口
interface DrawApi {

    public function drawCircle($radius, $x, $y);

}
```

```php
<?php
// 红色圆实现类
class RedCircle implements DrawApi {

    public function drawCircle($radius, $x, $y) {
        printf("Drawing Circle [color:red, radius:%s, x:%s, y:%s]\n", $radius, $x, $y);
    }

}
```

```php
<?php
// 绿色圆实现类
class GreenCircle implements DrawApi {

    public function drawCircle($radius, $x, $y) {
        printf("Drawing Circle [color:green, radius:%s, x:%s, y:%s]\n", $radius, $x, $y);
    }

}
```

```php
<?php
// 形状抽象类
abstract class Shape {

    protected $drawApi;

    public function __construct($drawApi) {
        $this->drawApi = $drawApi;
    }

    abstract public function draw();

}
```

```php
<?php
// 圆形类
class Circle extends Shape {

    public $radius;

    public $x;

    public $y;

    public function __construct($radius, $x, $y, DrawApi $drawApi) {
        parent::__construct($drawApi);
        $this->radius = $radius;
        $this->x = $x;
        $this->y = $y;
    }

    public function draw() {
        $this->drawApi->drawCircle($this->radius, $this->x, $this->y);
    }

}
```

```php
<?php
// 运行实例
$redCircle = new Circle(10, 100, 100, new RedCircle());
$redCircle->draw();

$greenCircle = new Circle(10, 100, 100, new GreenCircle());
$greenCircle->draw();
```

### 过滤器模式（Filter/ Criteria Pattern）

> 过滤器模式（Filter Pattern）或标准模式（Criteria  Pattern）是一种设计模式，这种模式允许开发人员使用不同的标准来过滤一组对象，通过逻辑运算以解耦的方式把它们连接起来。这种类型的设计模式属于结构型模式，它结合多个标准来获得单一标准。

```php
<?php
// 人员对象
class Person {

    protected $name;

    protected $gender;

    protected $maritalStatus;

    public function __construct($name, $gender, $maritalStatus) {
        $this->name = $name;
        $this->gender = $gender;
        $this->maritalStatus = $maritalStatus;

    }

    public function getName() {
        return $this->name;
    }

    public function getGender() {
        return $this->gender;
    }

    public function getMaritalStatus() {
        return $this->maritalStatus;
    }

}
```

```php
<?php
// 标准类
interface Criteria {

    public function meetCriteria(array $persons);

}
```

```php
<?php
// 男性标准类
class CriteriaMale implements Criteria {

    public function meetCriteria(array $persons): array {

        $result = [];
        foreach ($persons as $person) {
            if ('MALE' === strtoupper($person->getGender())) {
                array_push($result, $person);
            }
        }

        return $result;
    }

}
```

```php
<?php
// 女性标准类
class CriteriaFemale implements Criteria {

    public function meetCriteria(array $persons): array {

        $result = [];
        foreach ($persons as $person) {
            if ('FEMALE' === strtoupper($person->getGender())) {
                array_push($result, $person);
            }
        }

        return $result;
    }

}
```

```php
<?php
// 单身标准类
class CriteriaSingle implements Criteria {

    public function meetCriteria(array $persons): array {

        $result = [];
        foreach ($persons as $person) {
            if ('SINGLE' === strtoupper($person->getMaritalStatus())) {
                array_push($result, $person);
            }
        }

        return $result;
    }

}
```

```php
<?php
// 标准类并且条件
class AndCriteria implements Criteria {

    protected $criteria;

    protected $otherCriteria;

    public function __construct(Criteria $criteria, Criteria $otherCriteria) {
        $this->criteria = $criteria;
        $this->otherCriteria = $otherCriteria;
    }

    public function meetCriteria(array $persons) {
        return $this->otherCriteria->meetCriteria($this->criteria->meetCriteria($persons));
    }
}
```

```php
<?php
// 标准类或者条件
class OrCriteria implements Criteria {

    protected $criteria;

    protected $otherCriteria;

    public function __construct(Criteria $criteria, Criteria $otherCriteria) {
        $this->criteria = $criteria;
        $this->otherCriteria = $otherCriteria;
    }

    public function meetCriteria(array $persons) {
        $result = $this->criteria->meetCriteria($persons);

        foreach($this->otherCriteria->meetCriteria($persons) as $item) {
            if (!in_array($item, $result)) {
                array_push($result, $item);
            }
        }

        return $result;
    }
}
```

```php
<?php
// 实例
$persons = [
    new Person('Robert', 'Male', 'Single'),
    new Person('John', 'Male', 'Married'),
    new Person('Laura', 'Female', 'Married'),
    new Person('Diana', 'Female', 'Single'),
    new Person('Mike', 'Male', 'Single'),
    new Person('Bobby', 'Male', 'Single'),
];

$male = new CriteriaMale();
$female = new CriteriaFemale();
$single = new CriteriaSingle();
$singleMale = new AndCriteria($single, $male);
$singleOrMale = new OrCriteria($single, $male);

var_dump('male', $male->meetCriteria($persons));
var_dump('female', $female->meetCriteria($persons));
var_dump('single', $single->meetCriteria($persons));
var_dump('singleMale', $singleMale->meetCriteria($persons));
var_dump('singleOrMale', $singleOrMale->meetCriteria($persons));
```

### 组合模式（Composite Pattern）

> 组合模式（Composite Pattern），又叫部分整体模式，是用于把一组相似的对象当作一个单一的对象。组合模式依据树形结构来组合对象，用来表示部分以及整体层次。这种类型的设计模式属于结构型模式，它创建了对象组的树形结构。
> 这种模式创建了一个包含自己对象组的类。该类提供了修改相同对象组的方式。
> 我们通过下面的实例来演示组合模式的用法。实例演示了一个组织中员工的层次结构。

```php
<?php
// 职位类
class Employee {

    protected $name;

    protected $dept;

    protected $salary;

    protected $subordinates = [];

    public function __construct($name, $dept, $sal) {
        $this->name = $name;
        $this->dept = $dept;
        $this->salary = $sal;
    }

    public function add(Employee $e) {
        $this->subordinates[] = $e;
    }

    public function remove(Employee $e) {
        $index = array_search($e, $this->subordinates);

        if (false !== $index) {
            unset($this->subordinates[$index]);
            $this->subordinates = array_values($this->subordinates);
        }
    }

    public function getSubordinates() {
        return $this->subordinates;
    }

}
```

```php
<?php
// 实例
$ceo = new Employee("John", "CEO", 30000);

$headSales = new Employee("Robert","Head Sales", 20000);
$headMarketing = new Employee("Michel","Head Marketing", 20000);

$salesExecutive1 = new Employee("Richard","Sales", 10000);
$salesExecutive2 = new Employee("Rob","Sales", 10000);

$clerk1 = new Employee("Laura","Marketing", 10000);
$clerk2 = new Employee("Bob","Marketing", 10000);


$ceo->add($headSales);
$ceo->add($headMarketing);

$headSales->add($salesExecutive1);
$headSales->add($salesExecutive2);

$headMarketing->add($clerk1);
$headMarketing->add($clerk2);


var_dump($ceo);
exit;
```

### 装饰器模式（Decorator Pattern）

### 外观模式（Decorator Pattern）

### 享元模式（Flyweight Pattern）

### 代理模式（Proxy Pattern）



## 行为型模式（Behavioral Patterns）

> 这些设计模式特别关心对象之间的通信。

### 责任链模式（Chain of Responsibility Pattern）

### 命令模式（Command Pattern）

### 解释器模式（Interpreter Pattern）

### 迭代器模式（Iterator Pattern）

### 中介者模式（Mediator Pattern）

### 备忘录模式（Memento Pattern）

### 观察者模式（Observer Pattern）

### 状态模式（State Pattern）

### 空对象模式（Null Object Pattern）

### 策略模式（Strategy Pattern）

### 模板模式（Template Pattern）

### 访问者模式（Visitor Pattern）

