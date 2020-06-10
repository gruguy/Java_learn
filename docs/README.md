## 面向对象基础
  面向对象编程，就是一种通过对象的方式，把现实世界映射到计算机模型的一种编程方法。
  在现实生活中，每一个实体就是一个对象，比如：一本书、一部电影、一个人等等。 人就是我们抽象出来的一个分类(Class)，也就是一个对象，具体到某个人，他就是人这个类的具体实现，叫做实例(Instance)。

  Class就像一个模板，Instance是通过它来创造出来，一个Class可以创建多个Instance，他们的属性类型相同，但属性值不同：

  ```java
    class Person {
      private String name; // 姓名
      private int age;  // 年龄
      private int gender; // 性别 1男 2女
    }
  ```
  如上例所示，一个class包含多个fields（字段），字段用来表示一个类的特征。上面的类Person，我们定义了三个字段 一个是String类型的name字段，另两个分别是int类型的age和gender。因此通过class，把一组数据汇集到一个对象上，实现了数据的封装。再来写一个Book类：
  ```java
    class Book {
      public String name;
      public String author;
      public String isbn;
      public double price;
    }
  ```
  上面的类Book有四个字段，请指出各个字段。

  创建实例，定义好了class，只是定义好了模板，要根据对象木板创建出真正的实例对象，必须用new操作符。
  ```java
    Person xiaoming = new Person();
  ```
  上面代码创建了一个实例，我们定义了一个引用类型的变量xiaoming来指向这个实例
### 方法
一个class可以包含多个field, 直接把field用public暴露给外部可能会破坏封装性。如下：
```java
  Person ming = new Person();
  ming.name = 'Xiao Ming';
  ming.age = -99; // age设置为负数
```
显然直接操作field会造成逻辑环混乱。为了避免外部代码直接去访问field，我们可以使用private修饰field，拒绝外部访问：
```java
  public class Main{
    public static void main(String[] args){
      Person ming = new Person();
      ming.name = 'Xiao Ming';
      ming.age = 12;
    }
  }
  class Person {
    private String name;
    private int age;
  }

```
运行上面代码会抛出异常：
```java
  Main.java:5: error: name has private access in Person
        ming.name = "Xiao Ming"; // 对字段name赋值
            ^
  Main.java:6: error: age has private access in Person
        ming.age = 12; // 对字段age赋值
            ^
2 errors
error: compilation failed
```
看，现在程序不允许直接访问带有private修饰符的字段，那我们如何修改字段的属性值呢？
我们需要使用方法(method)来让外部代码可以间接修改field：
```java
public class Main {
  public static void mian(String[] args) {
    Person ming = new Person();
    ming.setName('Xiao Ming');
    ming.setAge(12);
    System.out.println(ming.getName()+","+ming.getAge());
  }
}

class Person {
  private String name;
  private int age;

  public String getName() {
    return this.name;
  }

  public void setName(String name){
    this.name = name;
  }

  public int getAge() {
    return this.age;
  }
  public void setAge(int age){
    if(age<0 || age> 100){
      throw IllegalArgumentException("invalid age value")
    }
    this.age = age;
  }
}
```
外部代码不能直接获取private字段，但可以通过getName()和getAge()间接获取字段的值，方法内部控制了程序逻辑正确一致。
#### this 变量
在方法内部，可以使用一个隐含的变量this，他始终指向当前实例。因此通过this.field就可以访问当前实例的字段。如果没有命名冲突，this可以省略。但不建议这样写
#### 方法参数
方法可以包含0个或者任意各参数。方法参数用于接收传递给方法的变量。调用方法时，必须严格按照参数的定义一一传递：
```java 
  class Person {
    ...
    public void setNameAndAge(String name, int age){
      ...
    }
  }
```
调用这个 setNameAndAge() 方法时，必须有两个参数，而且第一个参数必须是String，第二个必须为int：

```java
Person ming = new Person();
ming.setNameAndAge("Xiao Ming"); //编译错误，参数个数不对
ming.setNameAndAge(12, "Xiao Ming"); // 编译错误，参数类型不对
```

#### 可变参数
可变参数用 类型...来定义，可变参数相当于数组类型：
```java
class Group {
  private String[] names;

  public void setNames(String... names){
    this.names = names;
  }
}
```
上面的setNames()定义了一个可变参数，调用时，如下写法：

```java
  Group g = new Group();
  g.setNames("Xiao Ming", "Xiao Hong", "Xiao Zhang"); 
  g.setNames("Xiao Ming", "Xiao Hong"); 
  g.setNames("Xiao Ming"); 
  g.setNames();

```
完全可以把可变参数改写成String[]类型：

```java
  class Group {
    private String[] names;
    public void setNames(String[] names) {
      this.names = names
    } 
  }
```
但是 ，调用方需要自己先构造String[],比较麻烦：

```java
  Group g = new Group();
  g.setNames(new String[] {"XiaoMing", "XiaoHong Kong"});
```
另一个问题是，调用方可以传入null：

```java 
Group g = new Group();
g.setNames(null);
```
而可变参数可以保证无法传入null，以为传入0各参数时，接收到的实际是一个空数组而不是 null。

#### 参数绑定
也就是给字段赋值的过程，略。

### 构造方法
创建实例的时候，我们经常需要同时初始化这个实例的字段：
```java
  Person ming = new Person();
  ming.setName("小明");
  ming.setAge(12);
```
初始化对象需要三行代码，如果忘了调用setName() 或者setAge(), 这个实例内部的状态就是不正确的。

能否在创建对象的时候就把内部字段全部初始化为合适的值？
这就是我们需要构造方法的原因：

```java 
  public class Main{
    public static void main(String[] agrs){
      Person p = new Person("Xiao Ming", 15);
      System.out.println(p.getName());
      System.out.println(p.getAge());
    }
  }

  class Person {
    private String name;
    private int age;

    public Person(String name, int age){
      this.name = name;
      this.age = age;
    }

    public String getName() {
      return this.name
    }

    public void setName(String name) {
      this.name = name;
    }

    public int getAge() {
      return this.age;
    }

    public void setAge(int age) {
      this.age = age
    }
  }
```
你可能知道了构造方法是特殊的方法，名称就是类名，构造方法的参数没有显示，在方法内部，也可编写任意语句。和普通的方法相比，构造方法没有返回值(也没有void),调用构造方法，必须用 new 操作符。其实每一个class都有构造方法，只要是使用 new 操作符实例化的class都有构造方法，如果没有定义构造方法，编译器会自动生成一个默认的构造方法，他没有参数，也没有执行语句：

```java 
class Person {
  public Person() {}
}
```
s所以，当我们自定义了一个构造方法，编译器就不自动穿件默认构造方法了。
### 方法重载
在一个类中，我们可以定义多个方法，如果有一系列方法，他们功能都相似，只有参数不同，name可以把这一组方法名做成同名方法。如:
```java
class Hello {
  public void hello() {
    System.out.println("Hello, world!")
  }
   public void hello(String name) {
    System.out.println("Hello, " + name + "!")
  }
   public void hello(String name, int age) {
     if(age < 18) {
      System.out.println("Hi," + name + "!")
     } else {
      System.out.println("Hello, "+ name +"!")
     }
  }
}
```
这种方法名相同，但各自的参数不同，称为方法重载(Overload)
注意： 方法重载的返回值类型通常都是相同的。它的目的是功能类似的方法使用同一名字，更容易记住，调用更方便简单。
### 继承
前面我们定义了person类，现在我们需要定义一个Student类，字段如下：
```java
class Student {
  private String name;
  private int age;
  private int Score;

  public String getName() {...}
  public void setName(String name) {...}
  public int getAge() {...}
  public void setAge(int age){...}
  public void setAge(int age) {...} 
  public int getScore() {...}
  public void setScore(int score){...}

}
```
仔细观察，发现Student类包含了Person类已有的字段和方法，只是多出了一个score字段和相应的get和set方法。如何不再Student中不重复代码？
也就是这里讲的继承。

继承是面向对象编程中非常强大的一种机制，可以服用代码，当我们让Student从Person 继承时，Student就获得了Person的所有功能， Java使用 extends关键字来实现继承：

```java 
class Person {
  private String name;
  private int age;

  public String getName(){...}
  public void setName(String name){...}
  public int getAge() {...}
  public void setAge(int age){...}
}

class Student extends Person {
  private int score;

  public int getScore(){...}
  public void setScore(int score){...}
}
```
课件，继承是的Student类只需要编写额外的功能，不需要重复代码。

> 注意：子类自动获取了父类的所有字段，严禁定义和父类重名的字段！

Java只允许一个class继承自一个类，一个类有且仅有一个父类。只有Object特殊，他们有父类。

#### protect
集成有个特点，就是子类无法访问父类的private字段或者private方法：

```java
class Person {
  private String name;
  private int age;
}

class Student extends Person {
  public String hello() {
    return "hello,"+ name; //编译错误，无法访问字段
  }
}
```
这就使得集成作用被削弱了。为了让子类可以访问父类的字段，我们需要把private改为protected。用 protected 修饰的字段可以被子类访问：
```java
class Person {
  protected String name;
  protected int age;
}

class Student extends Person {
  public String hello(){
    return "hello," + name; // 正确访问！
  }
}
```
因此， protect 关键字可以把字段和方法的访问权限控制在继承树内部，一个 protected 字段和方法可以被其子类以及子类的子类所访问。

#### super
super 关键字表示父类（超类）。子类引用父类的字段时，可以使用 super.fieldName 如：
```java 
class Student extends Person {
  public String hello {
    return "Hello,"+ super.name
  }
}
```
实际上，这里的super.name或者this.name 以及 name 效果都是一样的，编译器会自动定位到父类的name字段。

但有的时候就必须使用super，如下：
```java 
public class Main{
  public static void main(String[] args){
    Student s = new Student("Xiao ming", 12, 90);
  }
}

class Person {
  protect String name;
  protect int age;

  public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }
}

class Student extends Person {
  protect int score;

  public Student(String name, int age, int score) {
    this.score = score;
  }
}
```

上面代码运行会编译错误，大意是在Student的构造方法中，无法调用Person的构造方法。

这是因为在Java中， 任何class的构造方法，第一行语句必须是要调用父类的构造方法。如果没有明确的调用父类构造方法，编译器会帮我们自动加上一句super(); 所以，Student类的构造方法实际上是这样：

```java
class Student extends Person {
  protected int score;

  public Student(String name , int age, int score) {
    super(); // 自动调用父类的构造方法
    this.score = score;
  }
}
```
但是，Person类并没有无参数的构造方法，编译也会失败。
解决方法是调用Person类存在的某个构造方法如下：
```java
class Student extends Person {
  protected int score;

  public Student(String name, int age, int score){
    super(name, age); // 调用父类构造方法
    this.score = score;
  }
}
```

> 如果父类没有默认构造方法，子类就必须显示调用super()并给出参数以便让编译器定位到父类的一个合适的构造方法。 子类不会继承任何父类的构造方法。子类默认的构造方法是编译期自动生成的，不是继承的。

#### 向上转型

如果一个引用变量的类型是Student，那么它可以指向一个Student类型的实例，类似，Person类也是。

Student是从Person继承下来的，那么一个引用类型的Person变量 能否只想Student实例？

```java
 Person p = new Student(); // 这是可以的
```
这是因为Student继承自Person。因此它拥有Person的全部功能。这种把子类类型安全的变为父类类型的赋值，被称为向上转型（upcasting）。

### 多态

### 抽象类

### 接口

### 静态字段和静态方法

### 包

### 作用域

### classpath和jar

### 模块

## Java核心类

### 字符串和编码

### StringBuilder

### StringJoiner

### 包装类型

### JavaBean

### 枚举类

### 记录类

### BigInteger

### BigDecimal

### 常用工具类
