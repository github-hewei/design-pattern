# 设计模式笔记

## 设计模式简介

设计模式（Design pattern）代表了最佳的实践，通常被有经验的面向对象的软件开发人员所采用。设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。

设计模式是一套被反复使用的，多数人知晓的，经过分类编目的，代码设计经验的总结。使用设计模式是为了重用代码，让代码更容易被他人理解。保证代码的可靠性。毫无疑问，设计模式于己于他人于系统都是多赢的，设计模式使代码编制真正工程化，设计模式是软件工程的基石，如同大厦的一块块砖石一样。项目中合理的运用设计模式可以完美的解决很多问题，每种模式在现实中都有相应的原理来与之对应，每种模式都描述了一个在我们周围不断重复发生的问题，以及该问题的核心解决方案，这也是设计模式能被广泛应用的原因。



## 创建型模式（Creational Patterns）

> 这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是直接使用new运算符直接实例化对象。这使得程序在判断针对给定某个实例需要创建哪些对象时更加灵活

### 工厂模式（Factory Pattern）

> 工厂模式（Factory Pattern）是 Java 中最常用的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
> 在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。

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

> 抽象工厂模式（Abstract Factory Pattern）是围绕一个超级工厂创建其他工厂。该超级工厂又称为其他工厂的工厂。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
> 在抽象工厂模式中，接口是负责创建一个相关对象的工厂，不需要显式指定它们的类。每个生成的工厂都能按照工厂模式提供对象。

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

> 单例模式（Singleton Pattern）是 Java 中最简单的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
> 这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

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
// 示例
Singleton::getInstance()->showMessage();
```

> Yii2 Framework 中对单例模式的应用

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

> 建造者模式（Builder Pattern）使用多个简单的对象一步一步构建成一个复杂的对象。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
> 一个 Builder 类会一步一步构造最终的对象。该 Builder 类是独立于其他对象的。

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

> 原型模式（Prototype Pattern）是用于创建重复的对象，同时又能保证性能。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
> 这种模式是实现了一个原型接口，该接口用于创建当前对象的克隆。当直接创建对象的代价比较大时，则采用这种模式。例如，一个对象需要在一个高代价的数据库操作之后被创建。我们可以缓存该对象，在下一个请求时返回它的克隆，在需要的时候更新数据库，以此来减少数据库调用。

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

> 适配器模式（Adapter Pattern）是作为两个不兼容的接口之间的桥梁。这种类型的设计模式属于结构型模式，它结合了两个独立接口的功能。
> 这种模式涉及到一个单一的类，该类负责加入独立的或不兼容的接口功能。举个真实的例子，读卡器是作为内存卡和笔记本之间的适配器。您将内存卡插入读卡器，再将读卡器插入笔记本，这样就可以通过笔记本来读取内存卡。

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

> 桥接（Bridge）是用于把抽象化与实现化解耦，使得二者可以独立变化。这种类型的设计模式属于结构型模式，它通过提供抽象化和实现化之间的桥接结构，来实现二者的解耦。
> 这种模式涉及到一个作为桥接的接口，使得实体类的功能独立于接口实现类。这两种类型的类可被结构化改变而互不影响。

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
> 装饰器模式（Decorator Pattern）允许向一个现有的对象添加新的功能，同时又不改变其结构。这种类型的设计模式属于结构型模式，它是作为现有的类的一个包装。
> 这种模式创建了一个装饰类，用来包装原有的类，并在保持类方法签名完整性的前提下，提供了额外的功能。

```php
<?php
// 图形抽象类
interface Shape {
    public function draw();
}
```

```php
<?php
// 长方形类
class Rectangle implements Shape {

    public function draw() {
        printf("Shape: Rectangle\n");
    }

}
```

```php
<?php
// 圆形类
class Circle implements Shape {

    public function draw() {
        printf("Shape: Circle\n");
    }

}
```

```php
<?php
// 实现了图形接口的图形抽象装饰器类
abstract class ShapeDecorator implements Shape {

    protected $shape;

    public function __construct($shape) {
        $this->shape = $shape;
    }

    public function draw() {
        $this->shape->draw();
    }

}
```

```php
<?php
// 扩展了图形抽象类的装饰器类
class RedShapeDecorator extends ShapeDecorator {

    public function __construct($shape) {
        parent::__construct($shape);
    }

    public function draw() {
        $this->shape->draw();
        $this->setRedBorder();
    }

    private function setRedBorder() {
        printf("Border Color: red\n");
    }

}
```

```php
<?php
// 示例
$circle = new Circle();
$circle->draw();

$redCircle = new RedShapeDecorator(new Circle());
$redCircle->draw();

$redRectangle = new RedShapeDecorator(new Rectangle());
$redRectangle->draw();
```

### 外观模式（Decorator Pattern）

> 外观模式（Facade Pattern）隐藏系统的复杂性，并向客户端提供了一个客户端可以访问系统的接口。这种类型的设计模式属于结构型模式，它向现有的系统添加一个接口，来隐藏系统的复杂性。
> 这种模式涉及到一个单一的类，该类提供了客户端请求的简化方法和对现有系统类方法的委托调用。

```php
<?php
// 图形接口类
interface Shape {

    public function draw();

}
```

```php
<?php
// 圆形类
class Circle implements Shape {

    public function draw() {
        printf("Circle::draw()\n");
    }

}
```

```php
<?php
// 长方形类
class Rectangle implements Shape {

    public function draw() {
        printf("Rectangle::draw()\n");
    }

}
```

```php
<?php
// 正方形类
class Square implements Shape {

    public function draw() {
        printf("Square::draw()\n");
    }

}
```

```php
<?php
// 绘制图形类的外观类
class ShapeMaker {

    protected $circle;

    protected $rectangle;

    protected $square;

    public function __construct() {
        $this->circle = new Circle();
        $this->rectangle = new Rectangle();
        $this->square = new Square();
    }

    public function drawCircle() {
        $this->circle->draw();
    }

    public function drawRectangle() {
        $this->rectangle->draw();
    }

    public function drawSquare() {
        $this->square->draw();
    }

}
```

```php
<?php
// 调用外观类
$shaleMaker = new ShapeMaker();

$shaleMaker->drawCircle();
$shaleMaker->drawRectangle();
$shaleMaker->drawSquare();

```

### 享元模式（Flyweight Pattern）

> 享元模式（Flyweight Pattern）主要用于减少创建对象的数量，以减少内存占用和提高性能。这种类型的设计模式属于结构型模式，它提供了减少对象数量从而改善应用所需的对象结构的方式。
> 享元模式尝试重用现有的同类对象，如果未找到匹配的对象，则创建新对象。

```php
<?php
// 图形接口类
interface Shape {

    public function draw();

}
```

```php
<?php
// 圆形类
class Circle implements Shape {

    protected $redius;

    protected $x;

    protected $y;

    protected $color;

    public function __construct($color) {
        $this->color = $color;
    }

    public function setX($x) {
        $this->x = $x;
    }

    public function setY($y) {
        $this->y = $y;
    }

    public function setRedius($redius) {
        $this->redius = $redius;
    }

    public function draw() {
        printf("Circle::draw() [color: %s, x:%s, y:%s, redius:%s]\n", $this->color, $this->x, $this->y, $this->redius);
    }

}
```

```php
<?php
// 图形对象工厂类
class ShapeFactory {

    public $circles = [];

    public function getCircle($color) {

        if (!isset($this->circles[$color])) {
            // echo "NEW\n";
            $this->circles[$color] = new Circle($color);
        }

        return $this->circles[$color];
    }

}
```

```php
<?php
// 示例
class Demo {

    public function main() {

        $shapeFactory = new ShapeFactory();

        for ($i = 0; $i < 20; $i++) {
            $circle = $shapeFactory->getCircle($this->getRandomColor());
            $circle->setX(mt_rand(0, 100));
            $circle->setY(mt_rand(0, 100));
            $circle->setRedius(100);
            $circle->draw();
        }

    }

    private function getRandomColor()
    {
        $colors = ['red', 'green', 'blue', 'yellow'];
        return $colors[array_rand($colors)];
    }
}

$demo = new Demo();
$demo->main();
```

### 代理模式（Proxy Pattern）

> 在代理模式（Proxy Pattern）中，一个类代表另一个类的功能。这种类型的设计模式属于结构型模式。
> 在代理模式中，我们创建具有现有对象的对象，以便向外界提供功能接口。

```php
<?php
// 图像接口类
interface Image {

    public function display();

}
```

```php
<?php
// 实现图像接口类
class RealImage implements Image {

    protected $fileName;

    public function __construct($fileName) {
        $this->fileName = $fileName;
        $this->loadFromDisk();
    }

    public function display() {
        printf("Displaying %s\n", $this->fileName);
    }

    public function loadFromDisk() {
        printf("Loading %s\n", $this->fileName);
    }

}
```

```php
<?php
// 代理图像接口类
class ProxyImage implements Image {

    private $realImage;

    private $fileName;

    public function __construct($fileName) {
        $this->fileName = $fileName;
    }

    public function display() {
        if ($this->realImage == null) {
            $this->realImage = new RealImage($this->fileName);
        }

        // 也可以在代理类中增加一些新的逻辑
        // 在不改变原有类的代码的同时，扩展一些新的功能，例如增加一层缓存
        {

        }

        $this->realImage->display();
    }

}
```

```php
<?php
// 实例
$image = new ProxyImage('123.jpg');

$image->display();
$image->display();
```

## 行为型模式（Behavioral Patterns）

> 这些设计模式特别关心对象之间的通信。

### 责任链模式（Chain of Responsibility Pattern）

> 顾名思义，责任链模式（Chain of Responsibility Pattern）为请求创建了一个接收者对象的链。这种模式给予请求的类型，对请求的发送者和接收者进行解耦。这种类型的设计模式属于行为型模式。
> 在这种模式中，通常每个接收者都包含对另一个接收者的引用。如果一个对象不能处理该请求，那么它会把相同的请求传给下一个接收者，依此类推。

```php
<?php
// 日志器抽象类
abstract class AbstractLogger {

    const INFO = 1;

    const DEBUG = 2;

    const ERROR = 3;

    protected $level;

    protected $nextLevelLogger;

    public function setNextLevelLogger($logger) {
        $this->nextLevelLogger = $logger;
    }

    public function logMessage($level, $message) {

        if ($this->level <= $level) {
            $this->write($message);
        }

        if ($this->nextLevelLogger != null) {
            $this->nextLevelLogger->logMessage($level, $message);
        }

    }

    abstract function write($message);

}
```

```php
<?php
class ConsoleLogger extends AbstractLogger {

    public function __construct($level) {
        $this->level = $level;
    }

    public function write($message) {
        printf("ConsoleLogger: %s\n", $message);
    }

}
```

```php
<?php
class FileLogger extends AbstractLogger {

    public function __construct($level) {
        $this->level = $level;
    }

    public function write($message) {
        printf("FileLogger: %s\n", $message);
    }

}
```

```php
<?php
class ErrorLogger extends AbstractLogger {

    public function __construct($level) {
        $this->level = $level;
    }

    public function write($message) {
        printf("ErrorLogger: %s\n", $message);
    }

}
```

```php
<?php
// 实例
class ChainPatternDemo {

    public function main() {

        $loggerChain = $this->getChainOfLoggers();

        $loggerChain->logMessage(AbstractLogger::ERROR, "Error Message");

        $loggerChain->logMessage(AbstractLogger::DEBUG, "Debug Message");

        $loggerChain->logMessage(AbstractLogger::INFO, "Info Message");

    }

    protected function getChainOfLoggers() {

        $consoleLogger = new ConsoleLogger(AbstractLogger::INFO);

        $fileLogger = new FileLogger(AbstractLogger::DEBUG);
        $fileLogger->setNextLevelLogger($consoleLogger);

        $errorLogger = new ErrorLogger(AbstractLogger::ERROR);
        $errorLogger->setNextLevelLogger($fileLogger);

        return $errorLogger;
    }

}

$demo = new ChainPatternDemo();
$demo->main();

// 输出
// ErrorLogger: Error Message
// FileLogger: Error Message
// ConsoleLogger: Error Message

// FileLogger: Debug Message
// ConsoleLogger: Debug Message

// ConsoleLogger: Info Message
```

### 命令模式（Command Pattern）

> 命令模式（Command Pattern）是一种数据驱动的设计模式，它属于行为型模式。请求以命令的形式包裹在对象中，并传给调用对象。调用对象寻找可以处理该命令的合适的对象，并把该命令传给相应的对象，该对象执行命令。

```php
<?php
// 订单接口类
interface Order {

    public function execute();

}
```

```php
<?php
// 创建一个请求类
class Stock {

    protected $name = 'abc';

    protected $quantity = 10;

    public function buy() {
        printf("Stock [Name: %s, Quantity: %d] bought\n", $this->name, $this->quantity);
    }

    public function sell() {
        printf("Stock [Name: %s, Quantity: %d] sold\n", $this->name, $this->quantity);
    }

}
```

```php
<?php
// 创建Order接口的实体类
class BuyStock implements Order {

    protected $abcStock;

    public function __construct($stock) {
        $this->abcStock = $stock;
    }

    public function execute() {
        $this->abcStock->buy();
    }

}
```

```php
<?php
// 创建Order接口的实体类
class SellStock implements Order {

    protected $abcStock;

    public function __construct($stock) {
        $this->abcStock = $stock;
    }

    public function execute() {
        $this->abcStock->sell();
    }
}
```

```php
<?php
// 创建命令调用类
class Broker {

    protected $orderList = [];

    public function takeOrder($order) {
        $this->orderList[] = $order;
    }

    public function placeOrders() {

        foreach($this->orderList as $order) {
            $order->execute();
        }

        $this->orderList = [];

    }
}
```

```php
<?php
// 示例
$abcStock = new Stock();

$buyStock = new BuyStock($abcStock);
$sellStock = new SellStock($abcStock);

$broker = new Broker();
$broker->takeOrder($buyStock);
$broker->takeOrder($sellStock);
$broker->takeOrder($sellStock);
$broker->placeOrders();
```

### 解释器模式（Interpreter Pattern）

> 解释器模式（Interpreter Pattern）提供了评估语言的语法或表达式的方式，它属于行为型模式。这种模式实现了一个表达式接口，该接口解释一个特定的上下文。这种模式被用在 SQL 解析、符号处理引擎等。

```php
<?php
interface Expression {

    public function interpret($context);

}
```

```php
<?php
class TerminalExpression implements Expression {

    protected $data;

    public function __construct($data) {
        $this->data = $data;
    }

    public function interpret($context) {
        if (false !== strpos($context, $this->data)) {
            return true;
        }

        return false;
    }

}
```

```php
<?php
class OrExpression implements Expression {

    protected $expr1;

    protected $expr2;

    public function __construct($expr1, $expr2) {
        $this->expr1 = $expr1;
        $this->expr2 = $expr2;
    }

    public function interpret($context) {
        return $this->expr1->interpret($context) || $this->expr2->interpret($context);
    }
}
```

```php
<?php
class AndExpression implements Expression {

    protected $expr1;

    protected $expr2;

    public function __construct($expr1, $expr2) {
        $this->expr1 = $expr1;
        $this->expr2 = $expr2;
    }

    public function interpret($context) {
        return $this->expr1->interpret($context) && $this->expr2->interpret($context);
    }
}
```

```php
<?php
// 示例
class InterpreterPatternDemo {

    // 规则 Robert 和 John 是男性
    public function getMaleExperssion() {
        $expr1 = new TerminalExpression('Robert');
        $expr2 = new TerminalExpression('John');

        return new OrExpression($expr1, $expr2);
    }

    // 规则 Julie 是一个已婚的女性
    public function getMarriedWomanExpression() {
        $expr1 = new TerminalExpression('Julie');
        $expr2 = new TerminalExpression('Married');

        return new AndExpression($expr1, $expr2);
    }

    public function main() {

        $isMale = $this->getMaleExperssion();
        printf("John is male? %s\n", $isMale->interpret('John') ? 'true' : 'false');

        $isMarriedWoman = $this->getMarriedWomanExpression();
        printf("Julie is warried woman? %s\n", $isMarriedWoman->interpret('Julie Married'));

    }
}

$demo = new InterpreterPatternDemo();
$demo->main();
```

### 迭代器模式（Iterator Pattern）
> 迭代器模式（Iterator Pattern）是 Java 和 .Net 编程环境中非常常用的设计模式。这种模式用于顺序访问集合对象的元素，不需要知道集合对象的底层表示。
> 迭代器模式属于行为型模式。

> 下面demo是运用php中自带的迭代器接口实现的

```php
<?php
class Person {
    protected $name;

    protected $age;

    public function __construct($name, $age) {
        $this->name = $name;
        $this->age = $age;
    }

    public function getName() {
        return $this->name;
    }
}
```

```php
<?php
class Persons implements Iterator{

    protected $personList = [];

    protected $iteratorIndex = 0;

    public function add($person) {
        $this->personList[] = $person;
    }

    public function current() {
        var_dump(__METHOD__);
        return $this->personList[$this->iteratorIndex];
    }

    public function key() {
        var_dump(__METHOD__);
        return $this->iteratorIndex;
    }

    public function next() {
        var_dump(__METHOD__);
        $this->iteratorIndex++;
    }

    public function rewind() {
        var_dump(__METHOD__);
        $this->iteratorIndex = 0;
    }

    public function valid() {
        var_dump(__METHOD__);
        return isset($this->personList[$this->iteratorIndex]);
    }
}
```

```php
<?php
// 使用
$persons = new Persons();
$persons->add(new Person('AAA', 111));
$persons->add(new Person('BBB', 222));
$persons->add(new Person('CCC', 333));
$persons->add(new Person('DDD', 444));

foreach ($persons as $person) {

    $name = $person->getName();
    var_dump($name);

}

echo "---\n";

foreach ($persons as $person) {

    $name = $person->getName();
    var_dump($name);

}
```

### 中介者模式（Mediator Pattern）

> 中介者模式（Mediator Pattern）是用来降低多个对象和类之间的通信复杂性。这种模式提供了一个中介类，该类通常处理不同类之间的通信，并支持松耦合，使代码易于维护。中介者模式属于行为型模式。

```php
<?php
class ChatRoom {

    public static function showMessage($user, $message) {
        printf("[%s] %s: %s\n", date('Y-m-d H:i:s'), $user->getName(), $message);
    }

}
```

```php
<?php
class User {

    protected $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function getName() {
        return $this->name;
    }

    public function setName($name) {
        $this->name = $name;
    }

    public function sendMessage($message) {
        ChatRoom::showMessage($this, $message);
    }

}
```

```php
<?php
// 实例
$john = new User('John');
$robert = new User('Robert');

$john->sendMessage('Hi, Robert !');
$robert->sendMessage('Hi, John !');
```

### 备忘录模式（Memento Pattern）

> 备忘录模式（Memento Pattern）保存一个对象的某个状态，以便在适当的时候恢复对象。备忘录模式属于行为型模式。

```php
<?php
class Memento {

    protected $state;

    public function __construct($state) {
        $this->state = $state;
    }

    public function getState() {
        return $this->state;
    }

}
```

```php
<?php
class Originator {

    protected $state;

    public function setState($state) {
        $this->state = $state;
    }

    public function getState() {
        return $this->state;
    }

    public function saveStateToMemento() {
        return new Memento($this->state);
    }

    public function getStateFromMemento($memento) {
        $this->state = $memento->getState();
    }

}
```

```php
<?php
class CareTaker {

    protected $mementoList = [];

    public function add($state) {
        $this->mementoList[] = $state;
    }

    public function get($index) {
        return $this->mementoList[$index]??null;
    }

}
```

```php
<?php
// 实例
$careTaker = new CareTaker();

$originator = new Originator();
$originator->setState('State #1');

$originator->setState('State #2');
$careTaker->add($originator->saveStateToMemento());

$originator->setState('State #3');
$careTaker->add($originator->saveStateToMemento());

$originator->setState('State #4');
printf("Current state: %s\n", $originator->getState());

$originator->getStateFromMemento($careTaker->get(1));
printf("Current state: %s\n", $originator->getState());

$originator->getStateFromMemento($careTaker->get(0));
printf("Current state: %s\n", $originator->getState());
```

### 观察者模式（Observer Pattern）

> 当对象间存在一对多关系时，则使用观察者模式（Observer Pattern）。比如，当一个对象被修改时，则会自动通知依赖它的对象。观察者模式属于行为型模式。

```php
<?php
class Subject {

    protected $observers = [];

    protected $state;

    public function getState() {
        return $this->state;
    }

    public function setState($state) {
        printf("State: %d\n", $state);
        $this->state = $state;
        $this->notifyAllObservers();
    }

    public function notifyAllObservers() {
        foreach($this->observers as $observer) {
            $observer->update();
        }
    }

    public function attach($observer) {
        $this->observers[] = $observer;
    }
}
```

```php
<?php
abstract class Observer {

    protected $subject;

    abstract public function update();
}
```

```php
<?php
class BinaryObserver extends Observer {

    protected $subject;

    public function __construct($subject) {
        $this->subject = $subject;
        $this->subject->attach($this);
    }

    public function update() {
        printf("Binary state: %s\n", decbin($this->subject->getState()));
    }
}
```

```php
<?php
class OctalObserver extends Observer {

    protected $subject;

    public function __construct($subject) {
        $this->subject = $subject;
        $this->subject->attach($this);
    }

    public function update() {
        printf("Octal state: %s\n", decoct($this->subject->getState()));
    }
}
```

```php
<?php
class HexaObserver extends Observer {

    protected $subject;

    public function __construct($subject) {
        $this->subject = $subject;
        $this->subject->attach($this);
    }

    public function update() {
        printf("Hexa state: %s\n", dechex($this->subject->getState()));
    }
}
```

```php
<?php
// 示例
$subject = new Subject();

new BinaryObserver($subject);
new OctalObserver($subject);
new HexaObserver($subject);

$subject->setState(8);

$subject->setState(4);

$subject->setState(100);
```

### 状态模式（State Pattern）

> 在状态模式（State Pattern）中，类的行为是基于它的状态改变的。这种类型的设计模式属于行为型模式。
> 在状态模式中，我们创建表示各种状态的对象和一个行为随着状态对象改变而改变的 context 对象。

```php
<?php
interface State {

    public function doAction($context);

}
```

```php
<?php
class StartState implements State {

    public function doAction($context) {
        printf("Player is in start state\n");
        $context->setState($this);
    }

    public function toString() {
        return "Start state";
    }

}
```

```php
<?php
class StopState implements State {

    public function doAction($context) {
        printf("Player is in Stop state\n");
        $context->setState($this);
    }

    public function toString() {
        return "Stop state";
    }

}
```

```php
<?php
class Context {

    protected $state;

    public function setState($state) {
        $this->state = $state;
    }

    public function getState() {
        return $this->state;
    }
}
```

```php
<?php
// 示例
$context = new Context();

$startState = new StartState();
$startState->doAction($context);
printf("%s\n", $context->getState()->toString());

$stopState = new StopState();
$stopState->doAction($context);
printf("%s\n", $context->getState()->toString());
```

### 空对象模式（Null Object Pattern）

> 在空对象模式（Null Object Pattern）中，一个空对象取代 NULL 对象实例的检查。Null 对象不是检查空值，而是反应一个不做任何动作的关系。这样的 Null 对象也可以在数据不可用的时候提供默认的行为。
> 在空对象模式中，我们创建一个指定各种要执行的操作的抽象类和扩展该类的实体类，还创建一个未对该类做任何实现的空对象类，该空对象类将无缝地使用在需要检查空值的地方。

```php
<?php
abstract class AbstractCustomer {

    protected $name;

    abstract public function isNil();

    abstract public function getName();
}
```

```php
<?php
class RealCustomer extends AbstractCustomer {

    public function __construct($name) {
        $this->name = $name;
    }

    public function isNil() {
        return false;
    }

    public function getName() {
        return $this->name;
    }
}
```

```php
<?php
class NullCustomer extends AbstractCustomer {

    public function getName() {

    }

    public function isNil() {
        return true;
    }
}
```

```php
<?php
class CustomerFactory {

    public static $names = ['John', 'Rob', 'Julie'];

    public static function getCustomer($name) {
        foreach (self::$names as $item) {
            if (false !== strpos($item, $name)) {
                return new RealCustomer($name);
            }
        }

        return new NullCustomer();
    }
}
```

```php
<?php
// 示例
$john = CustomerFactory::getCustomer('John');
if (!$john->isNil()) {
    var_dump($john->getName());
}

$rob = CustomerFactory::getCustomer('Rob');
if (!$rob->isNil()) {
    var_dump($rob->getName());
}

$julie = CustomerFactory::getCustomer('Julie');
if (!$julie->isNil()) {
    var_dump($julie->getName());
}

$laura = CustomerFactory::getCustomer('Laura');
if (!$laura->isNil()) {
    var_dump($laura->getName());
}
```

### 策略模式（Strategy Pattern）

> 在策略模式（Strategy Pattern）中，一个类的行为或其算法可以在运行时更改。这种类型的设计模式属于行为型模式。
> 在策略模式中，我们创建表示各种策略的对象和一个行为随着策略对象改变而改变的 context 对象。策略对象改变 context 对象的执行算法。

```php
<?php
interface Strategy {

    public function doOperation($num1, $num2);

}
```

```php
<?php
class OperationAdd implements Strategy {

    public function doOperation($num1, $num2) {
        return $num1 + $num2;
    }

}
```

```php
<?php
class OperationSubtract implements Strategy {

    public function doOperation($num1, $num2) {
        return $num1 - $num2;
    }

}
```

```php
<?php
class OperationMultiply implements Strategy {

    public function doOperation($num1, $num2) {
        return $num1 * $num2;
    }

}
```

```php
<?php
class Context {

    protected $strategy;

    public function __construct($strategy) {
        $this->strategy = $strategy;
    }

    public function executeStrategy($num1, $num2) {
        return $this->strategy->doOperation($num1, $num2);
    }
}
```

```php
<?php
// 示例
$context = new Context(new OperationAdd());
printf("1 + 2 = %d\n", $context->executeStrategy(1, 2));

$context = new Context(new OperationSubtract());
printf("10 - 2 = %d\n", $context->executeStrategy(10, 2));

$context = new Context(new OperationMultiply());
printf("2 * 8 = %d\n", $context->executeStrategy(2, 8));
```

### 模板模式（Template Pattern）

> 在模板模式（Template Pattern）中，一个抽象类公开定义了执行它的方法的方式/模板。它的子类可以按需要重写方法实现，但调用将以抽象类中定义的方式进行。这种类型的设计模式属于行为型模式。

```php
<?php
abstract class Game {

    abstract public function initialize();

    abstract public function startPlay();

    abstract public function endPlay();

    // 增加 final 关键字防止方法被重写
    final public function play() {

        $this->initialize();

        $this->startPlay();

        $this->endPlay();

    }
}
```

```php
<?php
class Cricket extends Game {

    public function initialize() {
        printf("Cricket initialize\n");
    }

    public function startPlay() {
        printf("Cricket start play\n");
    }

    public function endPlay() {
        printf("Cricket end play\n");
    }
}
```

```php
<?php
class Football extends Game {

    public function initialize() {
        printf("Football initialize\n");
    }

    public function startPlay() {
        printf("Football start play\n");
    }

    public function endPlay() {
        printf("Football end play\n");
    }
}
```

```php
<?php
// 示例
$cricket = new Cricket();
$cricket->play();

$football = new Football();
$football->play();
```

### 访问者模式（Visitor Pattern）

> 在访问者模式（Visitor Pattern）中，我们使用了一个访问者类，它改变了元素类的执行算法。通过这种方式，元素的执行算法可以随着访问者改变而改变。这种类型的设计模式属于行为型模式。根据模式，元素对象已接受访问者对象，这样访问者对象就可以处理元素对象上的操作。

```php
<?php
interface ComputerPart {

    public function accept($computerPartVisitor);

}
```

```php
<?php
class Keyboard implements ComputerPart {

    public function accept($computerPartVisitor) {
        $computerPartVisitor->visitKeyboard($this);
    }

}
```

```php
<?php
class Mouse implements ComputerPart {

    public function accept($computerPartVisitor) {
        $computerPartVisitor->visitMouse($this);
    }

}
```

```php
<?php
class Monitor implements ComputerPart {

    public function accept($computerPartVisitor) {
        $computerPartVisitor->visitMonitor($this);
    }

}
```

```php
<?php
class Computer implements ComputerPart {

    protected $parts = [];

    public function __construct() {
        $this->parts = [new Keyboard(), new Mouse(), new Monitor()];
    }

    public function accept($computerPartVisitor) {

        foreach($this->parts as $part) {
            $part->accept($computerPartVisitor);
        }

        $computerPartVisitor->visitComputer($this);
    }

}
```

```php
<?php
    interface ComputerPartVisitor {

    public function visitKeyboard($keyboard);

    public function visitMouse($mouse);

    public function visitMonitor($monitor);

    public function visitComputer($computer);

}
```

```php
<?php
class ComputerPartDisplayVisitor implements ComputerPartVisitor {

    public function visitComputer($computer) {
        printf("Displaying Computer.\n");
    }

    public function visitKeyboard($keyboard) {
        printf("Displaying Keyboard.\n");
    }

    public function visitMouse($mouse) {
        printf("Displaying Mouse.\n");
    }

    public function visitMonitor($monitor) {
        printf("Displaying Monitor.\n");
    }

}
```

```php
<?php
$computer = new Computer();
$computer->accept(new ComputerPartDisplayVisitor());
```

