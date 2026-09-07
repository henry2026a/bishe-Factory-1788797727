# 设计模式之工厂 Factory

> **发布时间**: 2026-09-03
> **标签**: 无

---

## 📞 联系方式
> 💬 **微信：sen232sen** | 🚀 **专业定制各类管理系统** | ⭐ **源码获取/技术支持**

---

任何可以产生对象的方法或者类，都可以称之为工厂

  * 灵活控制生产过程
  * 权限、修饰、日志…

### 工厂方法

工厂方法必备的要素：抽象产品、具体产品、抽象工厂、具体工厂  
具体实现：  
首先我们来定义一个抽象产品：即公用的类Animal

    public  abstract class Animal {
        abstract void shout();
    }

然后有具体的类来集成并实现Animal

    public class Cat extends Animal{
    
        public void shout(){
            System.out.println("喵喵");
        }
    }

    public class Dog extends Animal{
        @Override
        void shout() {
            System.out.println("汪汪");
        }
    }

定义抽象的工厂，注意抽象工厂的返回类一定要是抽象产品，这样才具有通用性

    public abstract class AnimalFactory {
        abstract Animal getAnimal();
    
    }

工厂利用的也是面向对象中的多态的特性  
然后我们来定义具体的工厂，生产具体的产品。这里要注意工厂也是有必要做成单例的，于是我们可以如下设计：

    public class DogFactory extends AnimalFactory{
        public static final DogFactory dogFactory = new DogFactory();
        @Override
        Animal getAnimal() {
            return new Dog();
        }
    
        private DogFactory(){}
    
        public static DogFactory getDogFactory(){
            return dogFactory;
        }
    }

主代码：

        public static void main(String[] args) {
            DogFactory dogFactory = DogFactory.getDogFactory();
            Animal animal = dogFactory.getAnimal();
            animal.shout();
        }

这样，在之后如果要创建Cat、Panda等类的时候，只需要去创建一个工厂，然后利用工厂调用即可，可以大大的减少代码的变动

### 抽象工厂

前面我们说了工厂方法，主要是用来创建单个的类，例如Dog、Cat  
但是如果现在我们需要创建一族的对象，怎么解决呢？如果继续使用工厂方法的话，那就需要创建许多的工厂，这显然比较麻烦。于是我们可以通过扩展抽象工厂来实现.  
抽象工厂的必备要素：抽象工厂、抽象类、具体工厂、具体类  
首先我们定义一个抽象工厂：

    public  abstract class AbstractFactory {
        abstract Food getFood();
    
        abstract Interest getInterest();
    
        abstract Water getWater();
    }

定义抽象类以及具体的实现类，这里就以Food为例了

    public abstract class Food {
        abstract void eat();
    }

    public class Bread extends Food{
        @Override
        void eat() {
            System.out.println("吃面包");
        }
    }

具体工厂：

    public class TomFactory extends AbstractFactory{
        private static final TomFactory TOM_FACTORY = new TomFactory();
    
        private TomFactory(){}
        @Override
        Food getFood() {
            return new Bread();
        }
    
        @Override
        Interest getInterest() {
            return new FootBall();
        }
    
        @Override
        Water getWater() {
            return new HotWater();
        }
    
        public static TomFactory getTomFactory(){
            return TOM_FACTORY;
        }
    }

main方法调用

        public static void main(String[] args) {
            TomFactory tomFactory = TomFactory.getTomFactory();
            Food food = tomFactory.getFood();
            Interest interest = tomFactory.getInterest();
            Water water = tomFactory.getWater();
            food.eat();
            interest.play();
            water.drink();
    
        }

以上就是抽象工厂。  
可以看到，抽象工厂的优势在于可以定义一族的对象，换言之，抽象工厂适合生产一类的对象，多个对象，但是不适合生产单个对象，假设我们只需要生产Food这一种对象，但是如果继承了抽象工厂，那就必须要实现他的所有方法，这显然是不符合需求的

---

![sensen](images/sensen.png)
