>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-30`*

## 概述

面向过程（POP）与面向对象（OOP）：

- 二者都是一种思想，面向对象是相对于面向过程而言的。

- 面向过程，强调的是功能行为，以函数为最小单位，考虑怎么做。面向对象，将功能封装进对象，强调具备了功能的对象，以类/对象为最小单位，考虑谁来做。

- 面向对象更加强调运用人类在日常的思维逻辑中采用的思想方法与原则，如抽象、分类、继承、聚合、多态等。

- 例如，人把大象装进冰箱：

  ![image-20210220095421669](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105052107-782123888.png)

面向对象的思想概述：

1. 程序员从面向过程的执行者转化成了面向对象的指挥者。
2. 面向对象分析方法分析问题的思路和步骤：
   - 根据问题需要，选择问题所针对的现实世界中的实体。
   - 从实体中寻找解决问题相关的属性和功能，这些属性和功能就形成了概念世界中的类。
   - 把抽象的实体用计算机语言进行描述，形成计算机世界中类的定义。即借助某种程序语言，把类构造成计算机能够识别和处理的数据结构。
   - 将类实例化成计算机世界中的对象。对象是计算机世界中解决问题的最终工具。

**Java 面向对象的三条主线：**

1. 类及类的成员：`属性`、`方法`、`构造器`、`代码块`、`内部类`。
2. 关键字：`instanceof`、`this`、`super`、`static`、`final`、`package`、`import`、`abstract`、`interface`等。
3. 面向对象的三大特征：`封装性（Encapsulation）`、`继承性（Inheritance）`、`多态性（Polymorphism）`、`（抽象性）`。

## 类和对象

**`类（Class）`**和**`对象（Object）`**是面向对象的核心概念。

- 类是对一类事物的描述，是抽象的、概念上的定义。

- 对象是实际存在的该类事物的每个个体，因而也称为`实例（instance）`。

### 类的定义

创建 Java 自定义类步骤：

1. 定义类：考虑修饰符、类名。
2. 编写类的属性：考虑修饰符、属性类型、属性名、初始化值。
3. 编写类的方法：考虑修饰符、返回值类型、方法名、形参等。
4. 按需编写类的构造器、代码块和内部类等。

**类的语法格式：**

```java
修饰符 class 类名 {
    属性声明;
    
    构造器声明;
    
    方法声明;
    
    代码块声明;
    
    内部类声明;
}
```

示例：

![image-20210220101937398](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105051282-1581878894.png)

**类的访问机制：**

- 在一个类中的访问机制：类中的方法可以直接访问类中的成员变量。例外：static 方法访问非 static 属性，编译不通过。

- 在不同类中的访问机制： 先创建要访问类的对象， 再用对象访问类中定义的成员。

### 对象的定义

对象创建的语法：

```java
类名 对象名 = new 类名();
```

使用`对象名.对象成员`的方式访问对象成员，包括属性和方法。示例：

```java
public class Animal {
    public int legs;

    public void eat() {
        System.out.println("Eating");
    }

    public void move() {
        System.out.println("Moving");
    }

    public static void main(String[] args) {
        // 创建对象
        Animal animal = new Animal();

        // 访问属性
        animal.legs = 4;
        System.out.println(animal.legs);

        // 访问方法
        animal.eat();

        // 访问方法
        animal.move();
    }
}
```

`匿名对象`：不定义对象的句柄，而直接调用这个对象的方法，这样的对象叫做匿名对象。例如："new Person().shout();"。如果对一个对象只需要进行一次方法调用，那么就可以使用匿名对象，我们经常将匿名对象作为实参传递给一个方法调用。

### 权限修饰符

`权限修饰符`：置于类的成员定义前，用来限定对象对该类成员的访问权限。权限从小到大排列：`private`、`缺省`（什么都不写）、`protected`、`public`。

![image-20210223173051273](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105050860-1066266317.png)

权限修饰符可以用来修饰类及类的内部结构：属性、方法、构造器、内部类。

- **对于 class 的权限修饰只可以用 public 和 default（缺省）。**
  - public 类可以在任意地方被访问。

  - default 类只可以被同一个包内部的类访问。

- **对于 class 的内部结构，四种权限修饰符都可以使用。**

总结：Java 提供了 4 种权限修饰符来修饰类及类的内部结构，体现类及类的内部结构在被调用时的可见性的大小。 

![image-20210223203339682](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105050476-1104649793.png)

### 属性（field）

语法格式：

```java
修饰符 数据类型 属性名 = 初始化值;
```

- 常用的权限修饰符有：private、缺省、protected、public。其他修饰符：static、final。

- 数据类型：任何基本数据类型（如 int、boolean 等）或任何引用数据类型。

- 属性名：属于标识符，符合命名规则和规范即可。

**属性（成员变量）与局部变量的区别：**

![image-20240930232914797](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105050112-622564413.png)

![image-20210220145307284](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105049752-603264510.png)

- **成员变量的默认初始化值：**当一个对象被创建时，会对其中各种类型的成员变量自动进行初始化赋值。除了基本数据类型之外的变量类型都是引用类型。

  ![image-20210220145909689](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105049425-444897128.png)

- **局部变量的默认初始化值：**局部变量声明后，没有默认初始化值，必须显式赋值，方可使用。特别的，形参在调用时，赋值即可。

**属性（成员变量）赋值的方式和先后顺序：**

- 赋值的方式：

  - ① `默认初始化`
  - ② `显示初始化`
  - ③ `构造器中初始化`
  - ④ `通过 "对象.属性" 或 "对象.方法" 的方式赋值`
  - ⑤ `在代码块中初始化`

- 赋值的先后顺序：**① - ② / ⑤ - ③ - ④**。

  - ② 和 ⑤，谁定义在前，谁先赋值：

    ```java
    public class Test {
        public static void main(String[] args) {
            Order order = new Order();
            System.out.println(order.orderId);// 4
        }
    }
    
    class Order {
        // 显示初始化在前
        int orderId = 3;
    
        // 代码块中初始化在后
        {
            orderId = 4;
        }
    }
    ```

    ```java
    public class Test {
        public static void main(String[] args) {
            Order order = new Order();
            System.out.println(order.orderId);// 3
        }
    }
    
    class Order {
        // 代码块中初始化在前
        {
            orderId = 4;
        }
    
        // 显示初始化在后
        int orderId = 3;
    }
    ```

- 属性（成员变量）赋值的执行顺序：

  ![image-20210301170915863](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105049104-1196633419.png)

### 方法（method）

#### 方法的定义

什么是方法（method 、函数）：

- 方法是类或对象行为特征的抽象，用来完成某个功能操作。在某些语言中也称为函数或过程。
- 将功能封装为方法的目的是，可以实现代码重用，简化代码。
- Java 里的方法不能独立存在，所有的方法必须定义在类里。

语法格式：

```java
修饰符 返回值类型 方法名([参数类型 形参, ...]) {
    方法体;
    retrun 返回值;
}
```

- 权限修饰符：public，缺省，private，protected 等。

- 返回值类型：
  - 没有返回值：使用 void。
  - 有返回值：在方法声明时，必须指定返回值的类型，同时，方法体中需要使用 return 关键字返回指定类型的变量或常量。

- 方法名 ：属于标识符，命名时遵循标识符命名规则和规范，能够见名知意。

- 形参列表：可以包含零个，一个或多个参数。多个参数时，中间用 "," 隔开。
- 方法体：方法功能的具体实现。

- 返回值：方法在执行完毕后返还给调用它的程序的数据。

方法的分类（按照是否有形参及返回值）：

![image-20210220161830762](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105048756-2113205642.png)

方法的调用：

- 方法通过方法名被调用，且只有被调用才会执行。

- 方法调用的过程：

  ![image-20210220162017612](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105048440-1890539875.png)

- 方法被调用一次，就会执行一次。

- 没有具体返回值的情况，返回值类型用关键字 void 表示，此时方法体中可以不必使用 return 语句。如果使用，表示用来结束方法。

- 定义方法时，方法的结果应该返回给调用者，交由调用者处理。

- 方法中可以调用当前类的属性或方法，不可以在方法内部定义方法。

#### 可变个数的形参

JavaSE 5.0 中提供了`Varargs（variable number of arguments）机制`，允许直接定义能和多个实参相匹配的形参。从而，可以用一种更简单的方式，来传递个数可变的实参。

![image-20210222201117424](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105048107-740025097.png)

语法格式：`方法名(参数的类型名... 参数名)`

可变参数的特点：

- 方法参数部分指定类型的参数个数是可变多个 ---> 0 个，1 个或多个。

- `可变个数形参的方法与同名的方法之间，彼此构成重载。`

- 可变参数方法的使用与方法参数部分使用数组是一致的，二者不共存。如下所示，方法二与方法三是相同的，不共存：

  ```java
  // 方法二
  public static void show(int... m) {
      // m 参数等同于数组，与数组的使用方法相同
      System.out.println(Arrays.toString(m));
  }
  
  // 方法三
  public static void show(int[] m) {
      System.out.println(m);
  }
  ```

- 方法的参数部分有可变形参，需要放在形参声明的最后。

  ```java
  // 合法
  public static void show(String str, int... m) {
      System.out.println(Arrays.toString(m));
  }
  
  // 不合法
  public static void show(int... m, String str) {
      System.out.println(Arrays.toString(m));
  }
  ```

- 在一个方法的形参位置，最多只能声明一个可变个数形参。

#### 方法参数的值传递机制

方法，必须由其所在类或对象调用才有意义。若方法含有参数：

- `形参`：方法声明时的参数。

- `实参`：方法调用时实际传给形参的数据。

Java 的实参值如何传入方法呢？Java 里方法的参数传递方式只有一种：**`值传递`**， 即将实际参数值的副本（复制品）传入方法内，而参数本身不受影响。

- **形参是基本数据类型：将实参基本数据类型变量的`"数据值"`传递给形参。**
- **形参是引用数据类型：将实参引用数据类型变量的`"地址值"`传递给形参。**

形参是基本数据类型与引用数据类型之间的区别：

```java
public class ValueTransferTest {
    public static void main(String[] args) {
        System.out.println("***************基本数据类型***************");
        int m = 10;
        int n = m;
        System.out.println("m = " + m + ", n = " + n);
        n = 20;
        System.out.println("m = " + m + ", n = " + n);

        System.out.println("***************引用数据类型***************");
        Order o1 = new Order();
        o1.orderId = 1001;
        Order o2 = o1;// 赋值后，o1 和 o2 的地址值相同，都指向了堆空间中的同一个实体
        System.out.println("o1.orderId = " + o1.orderId + ", o2.orderId = " + o2.orderId);
        o2.orderId = 1002;
        System.out.println("o1.orderId = " + o1.orderId + ", o2.orderId = " + o2.orderId);
    }
}

class Order {
    int orderId;
}
输出结果：
***************基本数据类型***************
m = 10, n = 10
m = 10, n = 20
***************引用数据类型***************
o1.orderId = 1001, o2.orderId = 1001
o1.orderId = 1002, o2.orderId = 1002
```

- **对于基本数据类型，两个不同方法内的局部变量，互不影响。**不因变量名相同而改变，因为是将实参基本数据类型变量的 "数据值" 传递给形参。

  ```java
  public class ValueTransferTest {
      public void swap(int m, int n) {
          System.out.println("swap方法中, 交换之前: m = " + m + ", n = " + n);
          int temp = m;
          m = n;
          n = temp;
          System.out.println("swap方法中, 交换之后: m = " + m + ", n = " + n);
      }
  
      public static void main(String[] args) {
          int m = 10;
          int n = 20;
          System.out.println("main方法中, 交换之前: m = " + m + ", n = " + n);
  
          // 能够交换 m 和 n 的值
          int temp = m;
          m = n;
          n = temp;
          System.out.println("main方法中, 交换之后: m = " + m + ", n = " + n);
  
          // 不能够交换 m 和 n 的值
          ValueTransferTest valueTransferTest = new ValueTransferTest();
          System.out.println("main方法中, 调用swap方法之前: m = " + m + ", n = " + n);
          valueTransferTest.swap(m, n);// // swap 方法调用完成后，该方法内的局部变量 temp，形参 m 和 n 从栈内存中弹出回收
          System.out.println("main方法中, 调用swap方法之后: m = " + m + ", n = " + n);
      }
  }
  输出结果：
  main方法中, 交换之前: m = 10, n = 20
  main方法中, 交换之后: m = 20, n = 10
  main方法中, 调用swap方法之前: m = 20, n = 10
  swap方法中, 交换之前: m = 20, n = 10
  swap方法中, 交换之后: m = 10, n = 20
  main方法中, 调用swap方法之后: m = 20, n = 10
  ```

- **对于引用数据类型，两个不同方法的局部变量，会互相影响。**因为是将实参引用数据类型变量的 "地址值" 传递给形参，二者指向的是堆内存中的同一个对象。

  ```java
  public class ValueTransferTest {
      public void swap(Data data) {
          System.out.println("swap方法中, 交换之前: data.m = " + data.m + ", data.n = " + data.n);
          int temp = data.m;
          data.m = data.n;
          data.n = temp;
          System.out.println("swap方法中, 交换之后: data.m = " + data.m + ", data.n = " + data.n);
      }
  
      public static void main(String[] args) {
          Data data = new Data();
          data.m = 10;
          data.n = 20;
          System.out.println("main方法中, 交换之前: data.m = " + data.m + ", data.n = " + data.n);
  
          // 能够交换 m 和 n 的值
          int temp = data.m;
          data.m = data.n;
          data.n = temp;
          System.out.println("main方法中, 交换之后: data.m = " + data.m + ", data.n = " + data.n);
  
          // 能够交换 m 和 n 的值
          ValueTransferTest valueTransferTest = new ValueTransferTest();
          System.out.println("main方法中, 调用swap方法之前: data.m = " + data.m + ", data. = " + data.n);
          valueTransferTest.swap(data);// swap 方法调用完成后，该方法内的局部变量 temp 和形参 data 从栈内存中弹出回收
          System.out.println("main方法中, 调用swap方法之后: data.m = " + data.m + ", data.n = " + data.n);
      }
  }
  
  class Data {
      int m;
      int n;java
  }
  输出结果：
  main方法中, 交换之前: data.m = 10, data.n = 20
  main方法中, 交换之后: data.m = 20, data.n = 10
  main方法中, 调用swap方法之前: data.m = 20, data. = 10
  swap方法中, 交换之前: data.m = 20, data.n = 10
  swap方法中, 交换之后: data.m = 10, data.n = 20
  main方法中, 调用swap方法之后: data.m = 10, data.n = 20
  ```

示例：

```java
public class ValueTransferTest {
    public void first() {
        int i = 5;
        Value v = new Value();
        v.i = 25;
        second(v, i);
        System.out.println(v.i);// 20
    }

    public void second(Value v, int i) {
        i = 0;
        v.i = 20;
        Value val = new Value();
        v = val;
        System.out.println(v.i + " " + i);// 15 0
    }

    public static void main(String[] args) {
        ValueTransferTest test = new ValueTransferTest();
        test.first();
    }
}

class Value {
    int i = 15;
}
```

示例二：

![image-20210223115107087](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105047743-502779165.png)

- 方法一：

  ```java
  public class Test {
      public static void method(int a, int b) {
          System.out.println("a = " + a * 10);
          System.out.println("b = " + b * 20);
          System.exit(0);
      }
  
      public static void main(String[] args) {
          int a = 10;
          int b = 10;
          method(a, b);
          System.out.println("a = " + a);
          System.out.println("b = " + b);
      }
  }
  ```

- 方法二：重写 PrintStream 的 println 方法。

  ```java
  public class Test {
      public static void method(int a, int b) {
          PrintStream printStream = new PrintStream(System.out) {
              @Override
              public void println(String x) {
                  if ("a = 10".equals(x)) {
                      x = "a = 100";
                  } else if ("b = 10".equals(x)) {
                      x = "b = 200";
                  }
                  super.println(x);
              }
          };
          System.setOut(printStream);
      }
  
      public static void main(String[] args) {
          int a = 10;
          int b = 10;
          method(a, b);
          System.out.println("a = " + a);
          System.out.println("b = " + b);
      }
  }
  ```

示例三：定义一个 int 型的数组`int[] arr = new int[]{12,3,3,34,56,77,432};`，让数组的每个位置上的值去除以首位置的元素，得到的结果，作为该位置上的新值，然后遍历新的数组。

```java
public class Test {
    public static void main(String[] args) {
        int[] arr = new int[]{12, 3, 3, 34, 56, 77, 432};
        System.out.println("计算前: " + Arrays.toString(arr));
        
        // 正确写法一
        int temp = arr[0];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = arr[i] / temp;
        }
        
        // 正确写法二
        for (int i = arr.length - 1; i >= 0; i--) {
            arr[i] = arr[i] / arr[0];
        }
        
        // 错误写法，因为第一次循环之后，首位置上的数已经改变，不再是最开始的值
        /*for (int i = 0; i < arr.length; i++) {
            arr[i] = arr[i] / arr[0];
        }*/
        
        System.out.println("计算后: " + Arrays.toString(arr));
    }
}
```

示例四：

```java
public class Test {
    public static void main(String[] args) {
        int[] arr = new int[]{1, 2, 3};
        System.out.println(arr);// 地址值

        char[] arr1 = new char[]{'a', 'b', 'c'};
        System.out.println(arr1);// 传入 char 数组时，println 方法体内是遍历这个数组
    }
}
输出结果：
[I@78308db1
abc
```

#### 方法的重载（overload）

概念：**在同一个类中，允许存在一个以上的同名方法，只要它们的参数个数或者参数类型不同即可。**

**方法重载的要求：**

- 与方法的权限修饰符、返回值类型、形参变量名、方法体都无关，`只看参数列表`，且参数列表（参数个数、参数类型或参数顺序）必须不同。调用时，根据方法参数列表的不同来区别。

>下面示例中，如果方法一不存在，main 方法依然正常执行，此时涉及到的是自动类型转换：
>
>```java
>// 方法一
>public static int getSum(int m, int n) {
>return m + n;
>}
>
>// 方法二
>public static double getSum(double m, double n) {
>return m + n;
>}
>
>public static void main(String[] args) {
>System.out.println(getSum(1, 2));
>}
>```

#### 方法的重写（override/overwrite）

概念：**在子类中可以根据需要，对从父类中继承来的方法进行改造，也称为方法的重置、覆盖。在程序执行时，子类重写的方法将覆盖父类的方法。**

**方法重写的要求：**

- 子类重写的方法必须和父类被重写的方法具有相同的方法名称、参数列表。
- 子类重写的方法使用的`访问权限`不能小于父类被重写的方法的访问权限（权限修饰符）。
  - 子类不能重写父类中声明为 private 权限的方法。
  - 子类中可以声明与父类 private 方法相同名称和参数列表的方法，但不属于重写。
- 子类重写的方法的`返回值类型`不能大于父类被重写的方法的返回值类型。

  - 父类被重写的方法的返回值类型是 void，则子类重写的方法的返回值类型只能是 void。
  - 父类被重写的方法的返回值类型是 A 类型，则子类重写的方法的返回值类型可以是 A 类或 A 类的子类。
  - 父类被重写的方法的返回值类型是基本数据类型（比如：double），则子类重写的方法的返回值类型必须是相同的基本数据类型（即，只能是 double）。
- 子类重写的方法`抛出的异常类型`不能大于父类被重写的方法抛出的异常类型。
- **`子类与父类中同名同参数的方法必须同时声明为非 static 的（此时属于重写），或者同时声明为 static 的（此时不属于重写）。因为 static 方法是属于类的，子类无法覆盖父类的方法。`**
  - 此时，子类中的该方法也须遵循重写关于访问权限、返回值和抛出异常类型等方面的规则。

**方法重载与重写的区别：**

1.  二者的定义细节：略。
2.  从编译和运行的角度看：重载，是指允许存在多个同名方法，而这些方法的参数不同。编译器根据方法不同的参数表，对同名方法的名称做修饰。对于编译器而言，这些同名方法就成了不同的方法。**它们的调用地址在编译期就绑定了。**Java 的重载是可以包括父类和子类的，即子类可以重载父类的同名不同参数的方法。所以：**对于重载而言，在方法调用之前，编译器就已经确定了所要调用的方法，这称为`"早绑定"`或`"静态绑定"`；而对于多态，只有等到方法调用的那一刻，解释运行器才会确定所要调用的具体方法，这称为`"晚绑定"`或`"动态绑定"`。**引用一句 Bruce Eckel 的话："不要犯傻，如果它不是晚绑定，它就不是多态。"
3.  **`重载不表现为多态性，重写表现为多态性。`**

#### 递归方法（recursion）

定义：一个方法体内调用它自身。方法递归包含了一种隐式的循环，它会重复执行某段代码，但这种重复执行无须循环控制。递归一定要向已知方向递归，否则这种递归就变成了无穷递归，类似于死循环。

示例：

```java
public class PassObject {
    // 1 ~ n 之间所有自然数的和
    public static int getSum(int n) {
        if (n == 1) {
            return 1;
        } else {
            return n + getSum(n - 1);
        }
    }

    // 1 ~ n 之间所有自然数的乘积
    public static long getProduct(int n) {
        if (n == 1) {
            return 1;
        } else {
            return n * getProduct(n - 1);
        }
    }

    public static void main(String[] args) {
        // 方式一：循环
        int sum = 0;
        for (int i = 1; i <= 100; i++) {
            sum += i;
        }
        System.out.println("1-100之间自然数的和: " + sum);

        // 方式二：递归
        System.out.println("1-100之间自然数的和: " + getSum(100));
        System.out.println("1-100之间自然数的积: " + getProduct(5));
    }
}
```

示例二：

```java
/**
 * 已知有一个数列: f(0) = 1，f(1) = 4，f(n+2) = 2 * f(n+1) + f(n)，其中 n 是大于 0 的整数，求 f(10) 的值
 */
public class PassObject {
    public static int f(int n) {
        if (n == 0) {
            return 1;
        } else if (n == 1) {
            return 4;
        } else {
            return 2 * f(n - 1) + f(n - 2);
        }
    }

    public static void main(String[] args) {
        int f = f(10);
        System.out.println(f);
    }
}
输出结果：
10497
```

示例三：

```java
/**
 * 已知一个数列: f(20) = 1，f(21) = 4，f(n+2) = 2 * f(n+1) + f(n)，其中 n 是大于 0 的整数，求 f(10) 的值
 */
public class PassObject {
    public static int f(int n) {
        if (n == 20) {
            return 1;
        } else if (n == 21) {
            return 4;
        } else {
            return f(n + 2) - 2 * f(n + 1);
        }
    }

    public static void main(String[] args) {
        int f = f(10);
        System.out.println(f);
    }
}
输出结果：
-3771
```

示例四：

```java
/**
 * 斐波那契数列：1 1 2 3 5 8 13 21 34 55 ...
 * 规律：一个数等于前两个数之和
 * 要求：计算斐波那契数列（Fibonacci）的第 n 个值，并将整个数列打印出来
 */
public class PassObject {
    public static int f(int n) {
        if (n <= 0 || n >= 30) {
            return 0;
        }
        if (n == 1) {
            return 1;
        } else if (n == 2) {
            return 1;
        } else {
            return f(n - 1) + f(n - 2);
        }
    }

    public static void main(String[] args) {
        int[] arr = new int[5];
        int sum = 0;
        for (int i = 0; i < arr.length; i++) {
            arr[i] = f(i + 1);
            sum += arr[i];
        }

        System.out.println(Arrays.toString(arr));
        System.out.println("和: " + sum);
    }
}
输出结果：
[1, 1, 2, 3, 5]
和: 12
```

示例五：

```java
public class PassObject {
    private static int count = 0;

    public static int recursion(int k) {
        count++;
        System.out.println("count1: " + count + ", k: " + k);
        if (k <= 0) {
            return 0;
        }
        return recursion(k - 1) + recursion(k - 2);
    }

    public static void main(String[] args) {
        recursion(4);
    }
}
输出结果：
count1: 1, k: 4
count1: 2, k: 3
count1: 3, k: 2
count1: 4, k: 1
count1: 5, k: 0
count1: 6, k: -1
count1: 7, k: 0
count1: 8, k: 1
count1: 9, k: 0
count1: 10, k: -1
count1: 11, k: 2
count1: 12, k: 1
count1: 13, k: 0
count1: 14, k: -1
count1: 15, k: 0
```

- 递归过程：遍历过程相当于二叉树的前序遍历。

  ![image-20210223160555518](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105047349-663360198.png)


###  构造器（constructor）

构造器，也叫构造方法。

作用：

1. 创建对象。如：`Order o = new Order();`。
2. 给对象进行初始化。如：`Person p = new Person(“Peter”, 15);`。

语法格式：

```java
修饰符 类名(参数列表) {
    初始化语句;
}
```

根据参数不同，构造器可以分为如下两类：

- 隐式无参构造器（系统默认提供）。

- 显式定义一个或多个构造器（无参、有参）。

构造器的特征：

- 构造器具有与类相同的名称，不声明返回值类型，与声明为 void 不同。
- Java 语言中，`每个类都至少有一个构造器`。
- **如果没有显示的定义类的构造器，则系统默认提供一个无参构造器。一旦显式定义了构造器， 则系统不再提供默认构造器。**
- 一般情况下，为了防止一些框架出异常，无论要不要自定义其他构造器，都应该把类的无参构造器显示的定义出来。
- **构造器的修饰符默认与所属类的修饰符一致，即：public 或 default（缺省）。**
- 构造器不能被 static、final、synchronized、abstract、native 修饰，不能有 return 语句返回值。
- 一个类中定义的多个构造器，彼此构成重载。
- 父类的构造器不可被子类继承。

### 代码块（或初始化块）

代码块的作用：对 Java 类或对象进行初始化。

代码块的分类：一个类中代码块若有修饰符，则只能被 static 修饰，称为`静态代码块（static block）`，没有使用 static 修饰的，为`非静态代码块`。

- **静态代码块：**
  - 内部可以有输出语句。
  - `随着类的加载而执行，而且只执行一次。`（不同于静态方法，静态方法必须在被类显示的调用后，才会执行方法内的语句。）
  - 作用：初始化类的信息。
  - 如果一个类定义了多个静态代码块，则按照声明的先后顺序来执行。一般情况下，不建议定义多个。
  - `静态代码块的执行要优先于非静态代码块的执行，与声明的先后顺序无关。`
  - 静态代码块中，只能调用静态的属性、静态的方法，不能调用非静态的属性、非静态的方法。

- **非静态代码块：**
  - 内部可以有输出语句。
  - `随着对象的创建而执行。`（不同于非静态方法，非静态方法必须在被类的对象显示的调用后，才会执行方法内的语句。）
  - `每创建一个对象，就执行一次非静态代码块，且先于构造器执行。`
  - 作用：可以在创建对象时，对对象的属性等进行初始化。
  - 如果一身上个类定义了多个非静态代码块，则按照声明的先后顺序来执行。一般情况下，不建议定义多个。
  - 非静态代码块中，可以调用静态的属性、静态的方法，也可以调用非静态的属性、非静态的方法。

代码块及构造器的执行顺序：

- **`由父及子，静态先行。`**

- **注意：调用 main() 方法时，需要先加载类，这个过程是早于 main() 方法中的首行执行语句的。**

- 示例：

  ```java
  class Root {
      static {
          System.out.println("Root的静态初始化块");
      }
  
      {
          System.out.println("Root的普通初始化块");
      }
  
      public Root() {
          System.out.println("Root的无参数的构造器");
      }
  }
  
  class Mid extends Root {
      static {
          System.out.println("Mid的静态初始化块");
      }
  
      {
          System.out.println("Mid的普通初始化块");
      }
  
      public Mid() {
          System.out.println("Mid的无参数的构造器");
      }
  
      public Mid(String msg) {
          // 通过 this 调用同一类中重载的构造器
          this();
          System.out.println("Mid的带参数构造器，其参数值：" + msg);
      }
  }
  
  class Leaf extends Mid {
      static {
          System.out.println("Leaf的静态初始化块");
      }
  
      {
          System.out.println("Leaf的普通初始化块");
      }
  
      public Leaf() {
          // 通过 super 调用父类中有一个字符串参数的构造器
          super("ABC");
          System.out.println("Leaf的构造器");
      }
  }
  
  public class LeafTest {
      public static void main(String[] args) {
          new Leaf();
          System.out.println();
          new Leaf();
      }
  }
  输出结果：
  Root的静态初始化块
  Mid的静态初始化块
  Leaf的静态初始化块
  Root的普通初始化块
  Root的无参数的构造器
  Mid的普通初始化块
  Mid的无参数的构造器
  Mid的带参数构造器，其参数值：ABC
  Leaf的普通初始化块
  Leaf的构造器
  
  Root的普通初始化块
  Root的无参数的构造器
  Mid的普通初始化块
  Mid的无参数的构造器
  Mid的带参数构造器，其参数值：ABC
  Leaf的普通初始化块
  Leaf的构造器
  ```

- 示例二：

  ```java
  class Father {
      static {
          System.out.println("11111111111");
      }
  
      {
          System.out.println("22222222222");
      }
  
      public Father() {
          System.out.println("33333333333");
  
      }
  
      // main 方法是一个静态方法，执行某个类的 main 方法之前，要先加载这个类，此处是 Father 类
      public static void main(String[] args) {
          System.out.println("77777777777");
          System.out.println("************************");
          new Son();
          System.out.println("************************");
  
          new Son();
          System.out.println("************************");
          new Father();
      }
  }
  
  class Son extends Father {
      static {
          System.out.println("44444444444");
      }
  
      {
          System.out.println("55555555555");
      }
  
      public Son() {
          System.out.println("66666666666");
      }
  
      // main 方法是一个静态方法，执行某个类的 main 方法之前，要先加载这个类，此处是先加载 Son 类
      public static void main(String[] args) { // 由父及子 静态先行
          System.out.println("77777777777");
          System.out.println("************************");
          new Son();
          System.out.println("************************");
  
          new Son();
          System.out.println("************************");
          new Father();
      }
  }
  
  public class Test {
      // main 方法是一个静态方法，执行某个类的 main 方法之前，要先加载这个类，此处是先加载 Test 类
      public static void main(String[] args) {
          System.out.println("77777777777");
          System.out.println("************************");
          new Son();
          System.out.println("************************");
  
          new Son();
          System.out.println("************************");
          new Father();
      }
  }
  ```

  - 调用 Father 类的 main() 方法，要先加载 Father 类。输出结果：

    ```java
    F: 11111111111
    F/m: 77777777777
    F/m: ************************
    S: 44444444444
    F: 22222222222
    F: 33333333333
    S: 55555555555
    S: 66666666666
    F/m: ************************
    F: 22222222222
    F: 33333333333
    S: 55555555555
    S: 66666666666
    F/m: ************************
    F: 22222222222
    F: 33333333333
    ```

  - 调用 Son 类的 main() 方法，要先加载 Son 类。输出结果：

    ```java
    F: 11111111111
    S: 44444444444
    S/m: 77777777777
    S/m: ************************
    F: 22222222222
    F: 33333333333
    S: 55555555555
    S: 66666666666
    S/m: ************************
    F: 22222222222
    F: 33333333333
    S: 55555555555
    S: 66666666666
    S/m: ************************
    F: 22222222222
    F: 33333333333
    ```

  - 调用 Test 类的 main() 方法，要先加载 Test 类。输出结果：

    ```java
    T/m: 77777777777
    T/m: ************************
    F: 11111111111
    S: 44444444444
    F: 22222222222
    F: 33333333333
    S: 55555555555
    S: 66666666666
    T/m: ************************
    F: 22222222222
    F: 33333333333
    S: 55555555555
    S: 66666666666
    T/m: ************************
    F: 22222222222
    F: 33333333333
    ```

### 内部类

当一个事物的内部，还有一个部分需要一个完整的结构进行描述，而这个内部的完整的结构又只为当前外部事物提供服务，那么整个内部的完整结构最好使用`内部类`。

- 在 Java 中，允许一个类 A 声明在另一个类 B 的内部，则类 A 称为内部类，类 B 称为外部类。

- Inner class一般用在定义它的类或语句块之内，在外部引用它时必须给出完整的名称。Inner class 的名字不能与包含它的外部类类名相同。

内部类的分类：成员内部类（静态的、非静态的），局部内部类（代码块内、构造器内、方法内）。

#### 成员内部类

成员内部类的特点：

- 一方面，作为外部类的成员：
  - 调用外部类的结构，注意生命周期，如静态成员内部类不能调用外部类非静态的方法。
  - 可以被 static 修饰，但此时就不能再使用外层类的非 static 的成员变量。注意，外部类不能被 static 修饰。
  - 可以被 private、protected、缺省和 public 四种权限修饰符修饰。注意，外部类不能被 private 和 protected 修饰。
- 另一方面，作为一个类：
  - 类内可以定义属性、方法、构造器、代码块、内部类等。
  - 可以被 final 修饰，表示此类不能被继承，如果不使用 final，就可以被继承。
  - 可以被 abstract 修饰，表示此类不能被实例化，可以被其它的内部类继承。
  - 编译以后生成 OuterClass$InnerClass.class 字节码文件（也适用于局部内部类）。
- 非 static 的成员内部类中的成员不能声明为 static 的，只有在外部类或 static 的成员内部类中才可声明 static 成员。
- 外部类访问成员内部类的成员，需要 "内部类.成员" 或 "内部类对象.成员" 的方式。
- 成员内部类可以直接使用外部类的所有成员，包括私有的数据。
- 当想要在外部类的静态成员部分使用内部类时，可以考虑内部类声明为静态的。

#### 局部内部类

局部内部类的特点：

- 局部内部类仍然是一个独立的类，在编译之后内部类会被编译成独立的 .class 文件，但是前面冠以外部类的类名和 $ 符号，以及数字编号。
- 只能在声明它的方法或代码块中使用，而且是先声明后使用，除此之外的任何地方都不能使用该类。
- 局部内部类的对象可以通过外部方法的返回值返回使用，返回值类型只能是局部内部类的父类或父接口类型。
- 局部内部类可以使用外部类的成员，包括私有的。
- `局部内部类可以使用外部方法的局部变量，但是必须是 final 的，final 可以省略（JDK 8 及之后），但这个局部变量赋值后不能有再次修改操作，否则编译不通过。`这是因为局部内部类和局部变量的声明周期不同所致。
- 局部内部类和局部变量地位类似，不能使用 public，缺省，protected 和 private 修饰。
- 局部内部类不能使用 static 修饰，因此也不能包含静态成员。

#### 匿名内部类

匿名内部类不能定义任何静态成员、方法和类，只能创建匿名内部类的一个实例。一个匿名内部类一定是在 new 的后面，用其隐含实现一个接口或实现一个类。

语法格式：

```java
new 父类构造器(实参列表)|实现接口() {
    匿名内部类的类体部分
}
```

匿名内部类的特点：

- 匿名内部类必须继承父类或实现接口。
- 匿名内部类只能有一个对象。
- 匿名内部类对象只能使用多态形式引用

#### 内部类的应用

关注如下的 3 个问题：

  - 如何实例化成员内部类的对象？

    - 静态成员内部类：`外部类.静态内部类 变量名 = new 外部类.静态内部类();`。
    - 非静态成员内部类：`外部类.非静态内部类 变量名 = new 外部类().new 非静态内部类();`。

  - 如何在成员内部类中区分调用外部类的结构？

    - 静态成员内部类，参考：

      ```java
      public void show(int age) {
          System.out.println("形参：" + age);
          System.out.println("静态成员内部类的静态属性：" + Brain.age);
          System.out.println("外部类的静态属性：" + Person.age);
      }
      ```

    - 非静态成员内部类，参考：

      ```java
      public void show(String name) {
          System.out.println("形参：" + name);
          System.out.println("非静态成员内部类的非静态属性：" + this.name);// 非静态成员内部类，不能定义 static 的变量
          System.out.println("外部类的非静态属性：" + Person.this.name);
      }
      ```

  - 开发中局部内部类的使用？

    ```java
    public class InnerClassTest1 {
        // 这种局部内部类，开发中很少见
        public void method() {
            class AA {
    
            }
        }
    
        // 返回一个实现类 Comparable 接口的类的对象
        public Comparable getComparable() {
            // 创建一个实现了 Comparable 接口的类：局部内部类
    
            // 方式一：创建 Comparable 接口的非匿名实现类的匿名对象
            /*class MyComparable implements Comparable {
                @Override
                public int compareTo(Object o) {
                    return 0;
                }
            }
    
            return new MyComparable();*/
    
            // 方式二：创建 Comparable 接口的匿名实现类的匿名对象
            return new Comparable() {
                @Override
                public int compareTo(Object o) {
                    return 0;
                }
            };
        }
    }
    ```

成员内部类和局部内部类应用示例：

```java
public class InnerClassTest {
    public static void main(String[] args) {
        // 1. 创建 Brain 实例---静态的成员内部类
        Person.Brain brain = new Person.Brain();
        brain.think();
        brain.show(8);

        // 2. 创建 Hand 实例---非静态的成员内部类
        Person.Hand hand = new Person().new Hand();
        hand.grasp();
        hand.show("外来手");
    }
}

class Person {
    String name = "小明";
    static int age = 8;

    // 静态成员内部类
    static class Brain {
        static int age = 8;

        public Brain() {

        }

        public void think() {
            System.out.println("大脑想东西");
        }

        public void show(int age) {
            System.out.println("形参：" + age);
            System.out.println("静态成员内部类的静态属性：" + Brain.age);
            System.out.println("外部类的静态属性：" + Person.age);
        }
    }

    // 非静态成员内部类
    class Hand {
        String name = "内部手";

        public Hand() {

        }

        public void grasp() {
            System.out.println("手抓东西");
            // 调用 Person 外部类的方法
            Person.this.eat();// 等价于 eat()，注意方法的生命周期
        }

        public void show(String name) {
            System.out.println("形参：" + name);
            System.out.println("非静态成员内部类的非静态属性：" + this.name);
            System.out.println("外部类的非静态属性：" + Person.this.name);
        }
    }

    static {
        // 静态代码块内局部内部类
        class AA {

        }
    }

    {
        // 非静态代码块内局部内部类
        class BB {

        }
    }

    public Person() {
        // 构造器内局部内部类
        class CC {

        }
    }

    public static void method1() {
        // 静态方法内局部内部类
        class DD {

        }
    }

    public void method() {
        // 非静态方法内局部内部类
        class EE {

        }
    }

    public void eat() {

    }
}
```

匿名内部类应用示例：

```java
interface Product {
    public double getPrice();

    
    public String getName();
}

public class AnonymousTest {
    public void test(Product p) {
        System.out.println("购买了一个" + p.getName() + "，花掉了" + p.getPrice());
    }

    public static void main(String[] args) {
        AnonymousTest ta = new AnonymousTest();
        // 调用 test 方法时，需要传入一个 Product 参数
        // 此处传入其匿名实现类的实例
        ta.test(new Product() {
            @Override
            public double getPrice() {
                return 567.8;
            }

            @Override
            public String getName() {
                return "AGP显卡";
            }
        });
    }
}
```

面试题：

```java
public class Test {
    public Test() {
        Inner s1 = new Inner();
        s1.a = 10;
        Inner s2 = new Inner();
        s2.a = 20;
        Test.Inner s3 = new Test.Inner();
        System.out.println(s3.a);
    }

    class Inner {
        public int a = 5;
    }

    public static void main(String[] args) {
        Test t = new Test();
        Inner r = t.new Inner();
        System.out.println(r.a);
    }
}
输出结果：
5
5
```

## 关键字

### instanceof

**`a instanceof A`**：检验对象 a 是否为类 A 的对象实例，如果是，返回 true，如果不是，返回 false。

使用情景：为了避免向下转型时出现 java.lang.ClassCastException，在向下转型之前，先进行 instanceof 判断，在返回 true 时，才进行向下转型。

- 要求 a 所属的类与类 A 必须是子类和父类的关系，否则编译错误。

- 如果 a 属于类 A 的子类 B，a instanceof A 的返回值也为 true。

  ![image-20210226172858254](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105046910-1803242936.png)

### this

**`this`**：

- this 理解为：当前对象或当前正在创建的对象。

- this 可以用来修饰或调用：属性、方法、构造器。

- this 修饰属性和方法：
  - **在类的方法中，可以使用 "this.属性" 或 "this.方法" 的方式，调用当前属性或方法。**
    - 通常情况下，可以省略 "this."。
    - 特殊情况下，如果方法的形参和类的属性同名，则必须显示的使用 "this.变量" 的方式，表明此变量是属性，而非形参。
  - **在类的构造器中，可以使用 "this.属性" 或 "this.方法" 的方式，调用当前正在创建的对象的属性或方法。**
    - 通常情况下，可以省略 "this."。
    - 特殊情况下，如果构造器的形参和类的属性同名，则必须显示的使用 "this.变量" 的方式，表明此变量是属性，而非形参。
  - **使用 this 访问属性和方法时，如果在本类中未找到，会从父类中查找。**
- this 调用构造器：
  - **在类的构造器中，可以显示的使用 "this(形参列表)" 的方式，调用本类中的其他构造器。**
    - 存在构造器的多重调用时，创建的对象仍然是只有一个，而不是调用一个构造器就创造了一个新的对象，只有最开始被调用的构造器才创造了对象。
  - 构造器中，不能使用 "this(形参列表)" 的方式调用自己。
  - **如果一个类中有 n 个构造器，则最多有 n - 1 个构造器中使用了 "this(形参列表)"。**
    - 构造器在彼此调用时，不能形成一个封闭环，如：构造器 A 中调用了构造器 B，则在构造器 B 中不能再调用构造器 A，多构造器调用类推。
  - **规定："this(形参列表)" 必须声明在当前构造器的首行。**
  - 一个构造器内部，最多只能声明一个 "this(形参列表)"，即只能调用一个其他的构造器。

示例：

  ```java
public class Person {
    private String name;

    private int age;

    // 无参构造器
    public Person() {
        this.eat();
    }

    // 带 name 的构造器
    public Person(String name) {
        this();// 调用无参构造器
        this.name = name;
    }

    // 带 name 和 age 的构造器
    public Person(String name, int age) {
        this(name);// 调用带 name 的构造器
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;// 此处 this 可以省略
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getAge() {
        return this.age;// 此处 this 可以省略
    }

    public void eat() {
        System.out.println("人吃饭");
        this.study();// this 调用方法，此处 this 可以省略
    }

    public void study() {
        this.eat();// this 调用方法，此处 this 可以省略
        System.out.println("人学习");
    }

    public static void main(String[] args) {

    }
}
  ```

示例二：

  ```java
public class Boy {
    private String name;

    private int age;

    public Boy() {

    }

    public Boy(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    public void marray(Girl girl) {
        System.out.println("我想娶" + girl.getName());
    }

    public void shout() {
        System.out.println("我想找对象");
    }
}
  ```

  ```java
public class Girl {
    private String name;

    private int age;

    public Girl() {

    }

    public Girl(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    public void marry(Boy boy) {
        System.out.println("我想嫁给" + boy.getName());
        boy.marray(this);// 传入当前 Girl 对象
    }

    public int compare(Girl girl) {
        return this.age - girl.age;
    }
}
  ```

  ```java
public class Person {
    public static void main(String[] args) {
        Boy boy = new Boy("罗密欧", 20);
        boy.shout();
        Girl girl = new Girl("朱丽叶", 18);
        girl.marry(boy);

        Girl girl2 = new Girl("祝英台", 19);
        System.out.println("年龄差：" + girl.compare(girl2));
    }
}
  ```

### super

**`super`**：

- super 理解为：父类的。
- super 可以用来调用父类的：属性、方法、构造器。
- **在子类的方法或构造器中，可以通过使用 "super.属性" 或 "super.方法" 的形式，显示的调用父类中声明的属性或方法。**
  - 通常情况下，可以省略 "super."。
  - 特殊情况：当子类和父类中定义了同名的属性时，要想在子类中调用父类中声明的该属性，则必须显示的使用 "super.属性" 的方式，表明调用的是父类中声明的属性。
  - 特殊情况：当子类重写了父类中的方法以后，要想在子类中调用父类中被重写的方法时，则必须显示的使用 "super.方法" 的方式，表明调用的是父类中被重写的方法。
- **在子类的构造器中，可以通过使用 "super(形参列表)" 的形式，显示的调用父类中声明的指定的构造器。**
  - "super(形参列表)" 的使用，必须声明在子类构造器的首行。
  - 在类的构造器中，针对于 "this(形参列表)" 或 "super(形参列表)"，只能二选一，不能同时出现。
  - **在构造器的首行，如果没有显示的声明 "this(形参列表)" 或 "super(形参列表)"，则默认调用的是父类中空参的构造器，即：`super();`。**
    - 子类中所有的构造器默认都会访问父类中空参的构造器。
    - 当父类中没有空参的构造器时，子类的构造器必须通过 "this(形参列表)" 或 "super(形参列表)" 语句，指定调用本类或者父类中相应的构造器。同时，只能二选一，且必须放在构造器的首行。
    - 如果子类构造器中既未显式调用父类或本类的构造器，且父类中又没有无参的构造器，则编译出错。
  - 在类的多个构造器中，至少有一个类的构造器中使用了 "super(形参列表)"，调用父类中的构造器。

**this 和 super 的区别：**

| 区别点     | this                                                     | super                                    |
| ---------- | -------------------------------------------------------- | ---------------------------------------- |
| 访问属性   | 访问本类中的属性，如果本类没有此属性，则从父类中继续查找 | 直接访问父类中的属性                     |
| 调用方法   | 访问本类中的方法，如果本类没有此方法，则从父类中继续查找 | 直接访问父类中的方法                     |
| 调用构造器 | 调用本类构造器，必须放在构造器的首行                     | 调用父类构造器，必须放在子类构造器的首行 |

思考：

- 为什么 "super(形参列表)" 和 "this(形参列表)" 调用语句不能同时在一个构造器中出现？
  - 因为 "super(形参列表)" 和 "this(形参列表)" 调用语句都必须出现在构造器中的首行。
- 为什么 "super(形参列表)" 和 "this(形参列表)" 只能作为构造器中的第一句出现？
  - 因为无论通过哪个构造器创建子类对象，都需要保证先初始化父类。这样做的目的是：当子类继承父类后，可以获得父类中所有的属性和方法，这样子类就有必要在一开始就知道父类是如何为对象进行初始化。

示例：

```java
public class Person {
    String name;
    int age;
    int id = 1000;

    public Person() {
        System.out.println("父类的空参构造器");
    }

    public Person(String name, int age, int id) {
        this.name = name;
        this.age = age;
        this.id = id;
    }

    public void eat() {
        System.out.println("吃饭");
    }

    public void sleep() {
        System.out.println("睡觉");
    }
}
```

```java
public class Student extends Person {
    // String name;// 父类中已有的属性，可以省略
    // int age;// 父类中已有的属性，可以省略
    String major;
    int id = 1001;

    public Student() {

    }

    public Student(String name, int age, String major) {
        this.name = name;
        this.age = age;
        this.major = major;
    }

    // 父类中已有的方法，可以省略，如有需要，可以重写
    // public void eat() {
    //     System.out.println("吃饭");
    // }

    // 重写父类的方法
    @Override
    public void sleep() {
        System.out.println("学生睡觉");
    }

    public void study() {
        System.out.println("学习");
    }

    public void show() {
        System.out.println("子类中的id: " + this.id);// this 可以省略，就近原则
        System.out.println("父类中的id: " + super.id);// 子类与父类有同名的属性 id，此时 super 不可以省略
    }

    public static void main(String[] args) {
        Student student = new Student();
        student.show();
    }
}
```

### static

当编写一个类时，其实就是在描述其对象的属性和行为，而并没有产生实质上的对象，只有通过 new 关键字才会产生出对象，这时系统才会分配内存空间给对象，其方法才可以供外部调用。有时候，希望无论是否产生了对象或无论产生了多少对象的情况下，`某些特定的数据在内存空间里只有一份`。例如：所有的中国人都有个国家名称，每一个中国人都共享这个国家名称，不必在每一个中国人的实例对象中都单独分配一个用于代表国家名称的变量。

实例变量：

```java
public class Test {
    public static void main(String[] args) {
        Circle c1 = new Circle(2.0); // c1.radius=2.0
        Circle c2 = new Circle(3.0); // c2.radius=3.0
    }
}

class Circle {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    public double findArea() {
        return Math.PI * radius * radius;
    }
}
```

- 上述代码中，c1 的 radius 独立于 c2 的 radius，存储在不同的空间。c1 中的 radius 变化不会影响 c2 的 radius，反之亦然。
- 像 Circle 类中的变量 radius 这样的，叫`实例变量 (instance variable)`，它属于类的每一个对象，不能被同一个类的不同对象所共享。
- 如果想让一个类的所有实例共享数据，就用`类变量`。类变量的定义，就需要用到`static`关键字。

**`static`**：

- static：静态的。

- static 可以用来修饰：属性、方法、代码块、内部类。

- static 修饰后的成员具备以下特点：

  - `随着类的加载而加载。`

  - `优先于对象存在。`
  - 修饰的成员，被所有对象所共享。
  - 访问权限允许时，可不创建对象，直接被类调用。

- 使用 static 修饰属性：`静态变量（类变量/class variable）`。

  - 属性，按是否使用 static 修饰，分为：静态属性和非静态属性（实例变量）。

    - 实例变量：当创建了类的多个对象，每个对象都独立的拥有一套类中的非静态属性。当修改其中一个对象的非静态属性时，不会导致其他对象中同样的属性值被修改。
    - 静态变量：当创建了类的多个对象，每个对象都共用同一个静态变量。当通过某一个对象修改静态变量时，会导致其他对象调用此静态变量时，是修改之后的值。 **注意：实际操作时，虽然编译能通过，但不应该通过类的实例对象来访问静态成员。**

    - `静态变量随着类的加载而加载`，可以通过`"类.静态变量"`的方式进行调用。
    - 静态变量的加载要早于对象的创建。实例变量在创建对象的过程中，或创建对象之后，才创建。
    - 由于类只会加载一次，则静态变量在内存中也只会存在一份：保存在`方法区的静态域`中。
    - 类可以访问静态变量，但不能访问实例变量（实例变量在对象产生时才生成），对象可以访问实例变量，也能访问静态变量（不推荐）。

  - 静态变量举例：System.out，Math.PI。

- 使用 static 修饰方法：`静态方法（类方法/class method）`。

  - `静态方法随着类的加载而加载`，可以通过`"类.静态方法"`的方式进行调用。

  - 类可以访问静态方法，但不能访问非静态方法（非静态方法在对象产生时才生成），对象可以访问非静态方法，也能访问静态方法（不推荐）。
  - **`静态方法中，只能调用静态属性或静态方法，它们的生命周期是一致的。非静态方法中，既可以调用非静态属性或非静态方法，也能调用静态属性或静态方法。`** 

- static 使用的注意点：

  - `在静态方法内，不能使用 this 关键字、super 关键字。`（this 和 super 指向当前类对象和父类对象，需要创建实例对象后才有这些概念。）

    ```java
    // 方法见下面的示例
    public static void show() {
        // 省略的是 Chiese.，而不是 this.
        walk();// 等同于 Chinese.walk();
        System.out.println("nation: " + nation);// 等同于 System.out.println(Chinese.nation);
    }
    ```

  - **`static 修饰的方法不能被重写。`**

  - 关于静态属性和静态方法的使用，从生命周期的角度去理解。

**类属性、类方法的设计思想：**

- 类属性作为该类各个对象之间共享的变量，在设计类时，分析哪些`属性不因对象的不同而改变`，将这些属性设置为类属性，相应的方法设置为类方法。
- 如果`方法与调用者无关`，则这样的方法通常被声明为类方法，由于`不需要创建对象就可以调用类方法`，从而简化了方法的调用。
- 类中的`常量`，通常也声明为 static 的。
- `操作静态属性的方法`，通常设置为 static 的。
- `工具类中的方法`，习惯上声明为 static 的。

示例：

```java
public class Test {
    public static void main(String[] args) {
        Chinese c1 = new Chinese();
        c1.name = "姚明";
        c1.age = 40;

        Chinese c2 = new Chinese();
        c2.name = "马龙";
        c2.age = 30;

        // 通过 c1 对象修改 nation 的值，c2 对象也能获得
        // 实际操作时，虽然编译能过通过，但不应该通过类的实例对象来访问静态成员
        c1.nation = "CHN";
        System.out.println(c2.nation);

        // 对象实例调用非静态方法
        c1.eat();

        // 类调用静态方法
        Chinese.show();

        // 通过 c1 对象也能调用非静态方法
        // 实际操作时，虽然编译能过通过，但不应该通过类的实例对象来访问静态成员
        c1.show();
    }
}

class Chinese {
    String name;
    int age;
    static String nation;

    public void eat() {
        System.out.println("吃饭");

        // 调用非静态结构
        this.info();
        System.out.println("name: " + this.name);

        // 调用静态结构
        walk();
        System.out.println("nation: " + nation);
    }

    public void info() {
        System.out.println("name: " + name + ", age: " + age + ", nation: " + nation);
    }

    public static void show() {
        System.out.println("我是中国人");

        // 不能调用非静态的结构
        // eat();
        // name = "Tom";

        // 调用静态的结构
        walk();
        System.out.println("nation: " + nation);// 省略的是Chiese.，而不是this.，等同于 System.out.println(Chinese.nation);
    }

    public static void walk() {
        System.out.println("走路");
    }
}
```

### final

final：

- 理解为：最终的。

- final 可以用来修饰：类、方法、变量（属性是成员变量，是变量的其中一种）。

- final 用来修饰类：`此类不能被其他类所继承`。例如：String 类、System 类、StringBuffer 类。

  ```java
  final class A {
      
  }
  
  // 错误，final 类不能被继承
  class B extends A {
      
  }
  ```

- final 用来修饰方法：`此方法不能被子类重写`。例如：Object 类中的 getClass()。

  ```java
  class A {
      public final void print() {
          System.out.println("A");
      }
  }
  
  class B extends A {
      // 错误，final 方法不能被子类重写
      public final void print() {
          System.out.println("A");
      }
  }
  ```

- final 用来修饰变量：`此时的 "变量" 称为常量，名称大写，且只能被赋值一次`。

  ```java
  public final class Test {
      public static int totalNumber = 5;
      
      public final int ID;
      
      public Test() {
          // 可以在构造器中给 final 修饰的 "变量" 赋值
          ID = ++totalNumber;
      }
      
      public static void main(String[] args) {
          Test t = new Test();
          System.out.println(t.ID);
          final int I = 10;
          final int J;
          J = 20;
          // 非法
          // J = 30;
      }
  }
  ```

  - `final 修饰成员变量`：必须在声明时或代码块中或在每个构造器中显式赋值，否则编译不通过。

    ```java
    public class FinalTest {
        // 1. 显式初始化：所有对象的这个常量值都是相同的，可以考虑直接显式初始化
        final int WIDTH = 0;
    
        // 2. 代码块中初始化：如果涉及到调用方法，或赋值操作较多，可以考虑代码块中初始化
        final int HEIGHT;
    
        {
            HEIGHT = show();
        }
    
        // 3. 构造器中初始化：如果涉及到调用方法，或赋值操作较多，可以考虑代码块中初始化
        final int LEFT;
    
        public FinalTest() {
            LEFT = show();
        }
    
        public int show() {
            return 0;
        }
    
        public static void main(String[] args) {
            FinalTest finalTest = new FinalTest();
        }
    }
    ```

  - `final 修饰局部变量`：修饰方法内局部变量时，表明该变量是一个常量，不能被修改；修饰形参时，表明此形参是一个常量，当调用此方法时，给常量形参赋一个实参，一旦赋值以后，就只能在方法体内使用此形参，但不能被修改。

    ```java
    public class FinalTest {
    
        public int show() {
            // 1. 修饰方法内局部变量：常量，不能被再次更改
            final int NUM = 10;
            return NUM;
        }
    
        // 2. 修饰形参：当方法被调用时，传入的实参，不能被再次更改
        public void show(final int num) {
            System.out.println(num);
        }
    
        public static void main(String[] args) {
            FinalTest finalTest = new FinalTest();
            finalTest.show(20);
        }
    }
    ```

- `static final`用来修饰属性：`全局常量`。

  ```java
  public class FinalTest {
  
      static final int WIDTH = 0;
  
      static final int HEIGHT;
  
      static {
          HEIGHT = show();
      }
      
      public FinalTest() {
  
      }
  
      public static int show() {
          return 0;
      }
  
      public static void main(String[] args) {
          FinalTest finalTest = new FinalTest();
      }
  }
  ```

面试题：

```java
public class Something {
    public int addOne(final int x) {
        // return ++x;// 编译不通过
        return x + 1;// 正常
    }
}
```

```java
public class Something {
    public void addOne(final Other o) {
        // o = new Other();// 编译不通过
        o.i++;// 正常
    }

    public static void main(String[] args) {
        Other o = new Other();
        new Something().addOne(o);
    }
}

class Other {
    public int i;
}
```

### package

**`package`**：作为 Java 源文件的第一条语句，指明该文件中定义的类所在的包。若缺省该语句，则指定为无名包。

语法格式：

```java
package 顶层包名.子包名;
```

- 包对应于文件系统的目录，package 语句中，用`.`来指明包（目录）的层次。

- 包属于标识符，遵循标识符的命名规范，通常用小写单词标识。通常使用所在公司域名的倒置，如：com.zeloud.xxx。

- 同一个包下，不能命名同名的接口、类。不同的包下，可以命名同名的接口、类。

JDK 中主要的包：

- `java.lang`：包含一些 Java 语言的核心类，如 String、Math、Integer、 System 和 Thread，提供常用功能。
- `java.net`：包含执行与网络相关的操作的类和接口。
- `java.io`：包含能提供多种输入/输出功能的类。
- `java.util`：包含一些实用工具类，如定义系统特性、接口的集合框架类、使用与日期日历相关的函数。
- `java.text`：包含了一些 Java 格式化相关的类。
- `java.sql`：包含了 Java 进行 JDBC 数据库编程的相关类/接口。
- `java.awt`：包含了构成抽象窗口工具集（abstract window toolkits）的多个类，这些类被用来构建和管理应用程序的图形用户界面（GUI）。（B/S 和 C/S）

### import

**`import`**：为使用定义在不同包中的 Java 类，需用 import 语句来引入指定包层次下所需要的类或全部类（.*）。import 语句告诉编译器到哪里去寻找类。

语法格式：

```java
import 包名.类名;
```

- 在源文件中使用 import 语句，可以显式的导入指定包下的类或接口。

- 声明在包的声明和类的声明之间。

- 如果需要导入多个类或接口，那么就并列显式声明多个 import 语句即可。

- 举例：可以使用 "import java.util.*;" 的方式，一次性导入 java.util 包下所有的类或接口。

- 如果导入的类或接口是 java.lang 包下的，或者是当前包下的，则可以省略此 import 语句。

- 如果在代码中使用不同包下的同名的类，那么使用时需要使用类的全类名的方式指明调用的是哪个类。

- 如果已经导入 java.a 包下的类，那么如果需要使用 a 包的子包下的类的话，仍然需要导入。

- `import static 组合`的使用：导入指定类或接口下的静态的属性或方法。

  ```java
  import static java.lang.System.*;
  
  public class Person {
      public static void main(String[] args) {
          out.println("打印方法");// 可以省略 System
      }
  }
  ```

## OOP 三大特征

### 封装性

**`封装性的设计思想：隐藏对象内部的复杂性，只对外公开简单的接口。`**便于外界调用，从而提高系统的可扩展性、可维护性。通俗的说，把该隐藏的隐藏起来，该暴露的暴露出来。

程序设计追求`"高内聚，低耦合"`：

- 高内聚：类的内部数据操作细节自己完成，不允许外部干涉。

- 低耦合：仅对外暴露少量的方法用于使用。

Java 中通过将对象的属性声明为私有的（private），再提供公共的（public）方法 ---> `getXxx()`和`setXxx()`，来实现对属性的操作，并以此达到`信息的封装和隐藏`的目的：

- 隐藏一个类中不需要对外提供的实现细节。

- 使用者只能通过事先定制好的方法来访问数据，可以方便地加入控制逻辑，限制对属性的不合理操作。

- 便于修改，增强代码的可维护性。


封装性的体现：`属性私有`、`方法私有`、`构造器私有`（如单例模式）等。

> 封装性的体现，需要权限修饰符的配合。

### 继承性

**如果多个类中存在相同的属性和行为时，将这些内容抽取到单独一个类中，那么这多个类无需再定义这些属性和行为，只要继承那个抽出来的类即可。**

此处的多个类称为`子类（派生类、subclass）`，单独的这个类称为`父类（基类、超类、superclass）`。可以理解为："子类 is a 父类"。

类继承语法格式：

```java
class SubClass extends SuperClass {
    
}
```

**继承性的作用：**

- 继承的出现减少了代码冗余，提高了代码的复用性。
- 继承的出现，更有利于功能的扩展。
- 继承的出现，让类与类之间产生了关系，提供了多态的前提。

**继承性的特点：**

- 子类继承了父类，就继承了父类中声明的所有属性和方法。**特别的，父类中声明为 private 的属性和方法，子类继承父类以后，仍然认为子类获取了父类中私有的结构，只是因为封装性的影响，使得子类的实例不能直接调用父类的这些私有的结构而已（事实上，父类的实例，也不能直接调用这些私有的结构）。**
- 在子类中，可以使用父类中定义的方法和属性，也可以声明创建子类特有的属性和方法，以实现功能的扩展。
- 在 Java 中，继承的关键字用的是`extends`，即子类不是父类的子集，而是对父类的扩展。

**继承性的规则：**

- 子类不能直接访问父类中私有的（private）的成员变量和方法。

- Java 只支持`单继承`和`多层继承`，不允许多重继承。

  - 一个子类只能有一个父类。

  - 一个父类可以派生出多个子类。

  - 此处强调的是 Java 类的单继承性，Java 中，接口是可以多继承的。

- 子类和父类是一个相对概念。子类直接继承的父类，称为直接父类，间接继承的父类，称为间接父类。

- 子类继承父类后，就获取了直接父类及所有间接父类中声明的属性和方法。

- 所有的 Java 类（除 java.lang.Object 类之外），都直接或间接继承 java.lang.Object。即，Java 中所有的类，都是 Object 类的子类。

### 多态性

**`多态性，也叫对象的多态性：父类的引用指向子类的对象（或子类的对象赋给父类的引用）。`**

- 一个变量只能有一种确定的数据类型。

- 一个引用类型变量可能指向（引用）多种不同类型的对象。

- 子类可看做是特殊的父类，所以父类类型的引用可以指向子类的对象：`向上转型（upcasting）`。

- 一个引用类型变量如果声明为父类的类型，但实际引用的是子类对象，那么该变量就不能再访问子类中添加的属性和方法：

  ![image-20210226115046078](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105046504-523109260.png)

多态的使用：

- **虚拟方法调用。**

- 有了对象的多态性以后，在编译期，只能调用父类中声明的方法，但在运行期，实际执行的是子类中重写的父类的方法。

- **`编译，看左边；运行，看右边。`**
  - Java 引用变量有两个类型：`编译时类型`和`运行时类型`。编译时类型由声明该变量时使用的类型决定，运行时类型由实际赋给该变量的对象决定。
  - `若编译时类型和运行时类型不一致，就出现了对象的多态性。`
  - 多态情况下，看左边：看的是父类的引用（父类中不具备子类特有的方法），看右边：看的是子类的对象（实际运行的是子类重写父类的方法）。

- `对象的多态性，只适用于方法，不适用于属性。`对于属性，编译期和运行期，看的都是左边，即都是父类中声明的那个属性。
  - 成员方法：编译时，要查看引用变量所声明的类中是否有所调用的方法。运行时，调用实际 new 的对象所属的类中的重写方法。
  - 成员变量：不具备多态性，只看引用变量所声明的类。

- 子类继承父类：

  - 若子类重写了父类方法，就意味着子类里定义的方法彻底覆盖了父类里的同名方法，系统将不可能把父类里的方法转移到子类中。
    - 编译，看左边；运行，看右边。

  - 对于实例变量则不存在这样的现象，即使子类里定义了与父类完全相同的实例变量，这个实例变量依然不可能覆盖父类中定义的实例变量。
    - 编译，运行，都看左边。

示例：

  ```java
public class Person {
    String name;
    int age;

    public void eat() {
        System.out.println("人吃饭");
    }

    public void  walk() {
        System.out.println("人走路");
    }

    public static void main(String[] args) {
        // 对象的多态性：父类的引用指向子类的对象
        Person person = new Man();
        // 多态的使用：当调用子父类同名同参数的方法时，实际执行的是子类重写的父类的方法--->虚拟方法调用
        // 编译期，只能调用父类 Person 类中的方法；运行期，执行的是子类 Man 类中的方法
        person.eat();
        person.walk();
        // 不能调用子类特有的属性或方法，因为编译时, person 是 Person 类型，而 Person 类中没有子类的这个特有属性或方法
        // 有了对象的多态性以后，内存中实际上是加载了子类特有的属性或方法的，但是由于变量声明为父类类型，导致编译时，只能调用父类中声明的属性和方法，子类中特有的属性和方法不能调用
        // person.isSmoking = true;
        // person.earnMoney();
        
        System.out.println("*********************************")
        // 如何才能使用子类特有的属性和方法？
        // 向下转型：使用强制类型转换符
        Man man = (Man) person;
        man.isSmoking = true;
        man.earnMoney();
        // 使用强转时，可能出现 java.lang.ClassCastException 异常
        Woman woman = (Woman) person;
        woman.goShopping();
    }
}
输出结果：
父类空参构造器
子类空参构造器
男人多吃肉，长肌肉
男人霸气的走路
*********************************
男人负责挣钱养家
  ```

  ```java
public class Man extends Person {
    boolean isSmoking;

    public void earnMoney() {
        System.out.println("男人负责挣钱养家");
    }

    @Override
    public void eat() {
        System.out.println("男人多吃肉，长肌肉");
    }

    @Override
    public void walk() {
        System.out.println("男人霸气的走路");
    }
}
  ```

示例二：

```java
public class FieldMethodTest {
    public static void main(String[] args) {
        Sub s = new Sub();
        System.out.println(s.count);// 20
        s.display();// 20
        Base b = s;
        // 对于引用数据，== 比较的是两个引用数据类型变量的地址值
        System.out.println(b == s);// true
        System.out.println(b.count);// 10
        b.display();// 20
    }
}

class Base {
    int count = 10;

    public void display() {
        System.out.println(this.count);
    }
}

class Sub extends Base {
    int count = 20;

    @Override
    public void display() {
        System.out.println(this.count);
    }
}
```

示例三：

```java
public class InterviewTest1 {
    public static void main(String[] args) {
        Base base = new Sub();
        base.add(1, 2, 3);// sub_1

        Sub s = (Sub) base;
        s.add(1, 2, 3);// sub_2
    }
}

class Base {
    public void add(int a, int... arr) {
        System.out.println("base");
    }
}

class Sub extends Base {
    @Override
    public void add(int a, int[] arr) {
        System.out.println("sub_1");
    }

    // 这个方法没有重写，在 Base 类中不存在这样声明的方法，也就没有多态，所以 base.add(1, 2, 3) 方法输出 sub_1
    public void add(int a, int b, int c) {
        System.out.println("sub_2");
    }
}
```

多态性的使用前提：

- **`有类的继承关系。`**
- **`有方法的重写。`**
- 如果没有以上两个前提，就不存在多态。

多态性的优点：

- 提高了代码的通用性，常称作`接口重用`。

- 方法声明的形参类型为父类类型，可以使用子类的对象作为实参调用该方法：

  ```java
  public class AnimalTest {
      // 多态的使用：传入的是 Animal 对象，但实际传入的可以是 Animal 的子类
      public void func(Animal animal) {
          animal.eat();
          animal.shout();
      }
  
      public static void main(String[] args) {
          AnimalTest animalTest = new AnimalTest();
  
          animalTest.func(new Dog());
          animalTest.func(new Cat());
      }
  }
  
  class Animal {
      public void eat() {
          System.out.println("动物：进食");
      }
  
      public void shout() {
          System.out.println("动物：叫");
      }
  }
  
  class Dog extends Animal {
      @Override
      public void eat() {
          System.out.println("狗吃骨头");
      }
  
      @Override
      public void shout() {
          System.out.println("汪！汪！汪！");
      }
  }
  
  class Cat extends Animal {
      @Override
      public void eat() {
          System.out.println("猫吃鱼");
      }
  
      @Override
      public void shout() {
          System.out.println("喵！喵！喵！");
      }
  }
  
  // 举例二
  class Order {
      // 此方法可以传入任意对象，而不需要每个特定对象都创建一次 method() 方法
      public void method(Object object) {
          // 方法体	
      }
  }
  ```

- 抽象类、接口的使用：`抽象类和接口不能实例化，它们的使用也体现了多态。`

虚拟方法调用： 

- 正常的方法调用：

  ```java
  Person e = new Person();
  e.getInfo();
  Student e = new Student();
  e.getInfo();
  ```

- 虚拟方法调用（多态情况下）：子类中定义了与父类同名同参数的方法，在多态情况下，将此时父类的方法称为**`虚拟方法`**，父类根据赋给它的不同子类对象，动态调用属于子类的该方法。这样的方法调用在编译期是无法确定的。

  ```java
  Person e = new Student();
  e.getInfo();// 调用 Student 类的 getInfo() 方法
  ```

- 编译时类型和运行时类型：上面代码中，编译时 e 为 Person 类型，而方法的调用是在运行时确定的，所以调用的是 Student 类的 getInfo() 方法 ---> **`动态绑定`**。

- **`重写是多态，重载不是。`**

- 示例：

  ![image-20210226131836066](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105046110-167762069.png)

多态是编译时行为还是运行时行为?

- **`多态是运行时行为`**，证明方法如下：

  ```java
  public class InterviewTest {
      public static Animal getInstance(int key) {
          switch (key) {
              case 0:
                  return new Cat();
              case 1:
                  return new Dog();
              default:
                  return new Sheep();
          }
      }
  
      public static void main(String[] args) {
          // 因为 key 需要在运行时才能得到值，编译期时无法判断 getInstance() 方法输出什么
          int key = new Random().nextInt(3);
          System.out.println(key);
          Animal animal = getInstance(key);
          animal.eat();
      }
  }
  
  class Animal {
      protected void eat() {
          System.out.println("animal eat food");
      }
  }
  
  class Cat extends Animal {
      @Override
      protected void eat() {
          System.out.println("cat eat fish");
      }
  }
  
  class Dog extends Animal {
      @Override
      public void eat() {
          System.out.println("Dog eat bone");
      }
  }
  
  class Sheep extends Animal {
      @Override
      public void eat() {
          System.out.println("Sheep eat grass");
      }
  }
  ```

### 抽象性

#### 抽象类和抽象方法

随着继承层次中一个个新子类的定义，类变得越来越具体，而父类则更一般，更通用。类的设计应该保证父类和子类能够共享特征。有时将一个父类设计得非常抽象，以至于它没有具体的实例，这样的类叫做抽象类。

抽象类应用：抽象类是用来模型化那些父类无法确定全部实现，而是由其子类提供具体实现的对象的类。

**`abstract`**：

- abstract：抽象的。
- abstract 可以用来修饰：类、方法。
  - abstract 不能修饰变量、代码块、构造器。
  - abstract 不能修饰私有方法、静态方法、final 的方法、final 的类。

- abstract 修饰类：`抽象类`。
  - `抽象类不能实例化`。
  - 抽象类中一定有构造器，便于子类实例化时调用，但抽象类本身不能使用构造器。
  - 开发中，会提供抽象类的子类，让子类对象实例化，完成相关操作。
- abstract 修饰方法：`抽象方法`。
  - 抽象方法只有方法声明，没有方法体，以分号结束。比如：public abstract void talk();。
  - 包含抽象方法的类，一定是一个抽象类。反之，`抽象类中可以没有抽象方法`。
  - 若子类重写了父类（不仅包括直接父类，也包括间接父类）中的`所有的`抽象方法后，此子类方可实例化；若子类没有重写父类中的所有的抽象方法，则此子类也是一个抽象类，需要使用 abstract 修饰。

#### 抽象类的匿名子类对象

```java
public class Test {
    public static void method(Student student) {

    }

    public static void method1(Person person) {
        person.eat();
    }

    public static void main(String[] args) {
        // 匿名对象
        method(new Student());

        // 1. 创建非匿名类的非匿名的对象
        Worker worker = new Worker();
        method1(worker);

        // 2. 创建非匿名类的匿名的对象
        method1(new Worker());

        // 3. 创建匿名子类的非匿名的对象：p
        Person p = new Person() {
            @Override
            public void eat() {
                // 重写方法体
                System.out.println("...");
            }
        };
        method1(p);

        // 4. 创建匿名子类的匿名对象
        method1(new Person() {
            @Override
            public void eat() {
                // 重写方法体
                System.out.println(",,,");
            }
        });
    }
}

abstract class Person {
    public abstract void eat();
}

class Student {

}

class Worker extends Person {
    @Override
    public void eat() {
        // 重写方法体
        System.out.println("、、、");
    }
}
```

#### 接口（interface）

一方面，有时必须从几个类中派生出一个子类，继承它们所有的属性和方法。但是，`Java 不支持多重继承`。有了接口，就可以得到多重继承的效果。另一方面，有时必须从几个类中抽取出一些共同的行为特征，而它们之间又没有 is - a 的关系，仅仅是具有相同的行为特征而已。例如：鼠标、键盘、打印机、扫描仪、摄像头、充电器、MP3 机、手机、数码相机、移动硬盘等都支持 USB 连接。

接口就是规范，定义的是一组规则，体现了现实世界中 "如果你是/要...则必须能..." 的思想。继承是一个 "是不是" 的关系，而接口实现则是 "能不能" 的关系。

接口的本质是契约，标准，规范，就像我们的法律一样，制定好后大家都要遵守。

接口的定义：

- 接口使用`interface`定义。

- 接口中`不能定义构造器`，意味着接口不可以实例化。

- 如何定义接口中的成员：

  - JDK 7 及以前：只能定义全局常量和抽象方法。

    - `全局常量`：接口中的所有成员变量都`默认由 public static final 修饰`的。书写时，可以省略，但含义不变，常量不能被更改。
    - `抽象方法`：接口中的所有抽象方法都`默认由 public abstract 修饰`的。

  - JDK 8：除了定义全局常量和抽象方法之外，还可以定义静态方法、默认方法。

    - `静态方法`：使用`static`关键字修饰，默认为 public 的。

      - 只能通过`接口直接调用`，并执行其方法体。

    - `默认方法`：使用`default`关键字修饰，默认为 public 的。

      - 可以通过`实现类的对象来调用`，如果实现类重写了接口中的默认方法，调用时，执行的是重写后的方法。

      - 如果子类（或实现类）继承的父类和实现的接口中，声明了同名同参数的默认方法，那么子类在没有重写此方法的情况下， 默认调用的是父类中的同名同参数的方法 ---> `类优先原则`。如果重写了，调用子类重写的方法。

        ![image-20220505082232062](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105045792-195910201.png)

      - 如果实现类实现了多个接口，而多个接口中定义了同名同参数的默认方法，那么在实现类没有重写此方法的情况下，编译不通过 ---> `接口冲突`。如果要避免接口冲突，则在实现类中，必须重写此方法。

        ![image-20220505082303861](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105045483-704874856.png)

      - 在子类（或实现类）的方法中，使用`super.方法名`调用父类的方法，使用`接口名.super.方法名`调用接口中的方法。

        ![image-20210303132422993](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105045142-1697955679.png)

    - 示例：

      ```java
      public interface InterfaceA {
          // 静态方法
          static void method1() {
              System.out.println("接口A：静态方法1");
          }
      
          // 默认方法
          default void method2() {
              System.out.println("接口A：默认方法2");
          }
      
          default void method3() {
              System.out.println("接口A：默认方法3");
          }
      
          default void method4() {
              System.out.println("接口A：默认方法4");
          }
      
          default void method5() {
              System.out.println("接口A：默认方法5");
          }
      }
      ```

      ```java
      public interface InterfaceB {
          default void method5() {
              System.out.println("接口B：默认方法5");
          }
      }
      ```

      ```java
      public class SubClassTest {
          public static void main(String[] args) {
              // 1. 静态方法
              InterfaceA.method1();
      
              SubClass subClass = new SubClass();
              // 2. 默认方法
              subClass.method2();
              // 3. 重写的默认方法
              subClass.method3();
              // 4. 调用的是父类中的 method4()
              subClass.method4();
          }
      }
      
      class SuperClass {
          public void method4() {
              System.out.println("父类：方法4");
          }
      }
      
      class SubClass extends SuperClass implements InterfaceA, InterfaceB {
          // 重写接口 InterfaceA 中的 method3()
          @Override
          public void method3() {
              System.out.println("实现类：方法3");
          }
      
          // 重写了父类 SuperClass 的 method4()
          @Override
          public void method4() {
              System.out.println("实现类：方法4");
          }
      
          // InterfaceA 和 InterfaceB 声明了同名同参的 method5()，SubClass 中必须重写此方法，否则接口冲突，编译不通过
          // 如果继承的父类 SuperClass 中也声明了同名同参的 method5()，则不会出现接口冲突
          @Override
          public void method5() {
              System.out.println("实现类：方法5");
          }
      
          public void myMethod() {
              method2();// InterfaceA 的method2()
      
              method3();// 重写的 InterfaceA 的 method3()
              InterfaceA.super.method3();// InterfaceA 的 method3()
      
              method4();// 重写的 SuperClass 的 method4()
              super.method4();// 父类 SuperClass 的 method4()
      
              InterfaceA.super.method5();// InterfaceA 的 method5()
              InterfaceB.super.method5();// InterfaceB 的 method5()
          }
      }
      ```

接口的使用：

- Java 中，接口和类是并列的两个结构，或者可以理解为一种特殊的类。从本质上讲，接口是一种特殊的抽象类。

- Java 中，接口都通过让类去实现的方式（`implements`）来使用（`面向接口编程`）。

- 如果实现类覆盖了接口中（包括直接接口和间接接口）的所有抽象方法，则此实现类可以实例化。如果实现类没有覆盖接口（包括直接接口和间接接口）中所有的抽象方法，则此实现类仍为一个抽象类。

  ```java
  interface MyInterface{
      String s = "MyInterface";
      public void absM1();
  }
  
  interface SubInterface extends MyInterface{
  	public void absM2();
  }
  
  // 实现类 SubAdapter 必须给出接口 SubInterface 以及父接口 MyInterface 中所有方法的实现
  // 否则，SubAdapter 仍需声明为 abstract 的
  public class SubAdapter implements SubInterface{
      @Override
      public void absM1(){
          System.out.println("absM1");
      }
      
      @Override
      public void absM2(){
          System.out.println("absM2");
      }
  }
  ```

- `Java 类可以实现多个接口`，弥补了 Java 类单继承性的局限性。

  - 格式：`class SubClass extends SuperClass implements InterfaceA, InterfaceB, InterfaceC {}`

- `接口与接口之间可以继承，而且可以多继承，使用关键字 extends`，但接口不能多实现接口。

  - 格式：`interface InterfaceA extends InterfaceB, InterfaceC {}`

- 与继承关系类似，接口与实现类之间体现了多态性。

- 接口，实际上可以看作是一种规范。

- 示例：

  ![image-20210303092504018](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105044785-1550630351.png)

  ```java
  public class InterfaceTest {
      public static void main(String[] args) {
          System.out.println(Flyable.MAX_SPEED);
          System.out.println(Flyable.MIN_SPEED);
  
          Plane plane = new Plane();
          plane.fly(); 
      }
  }
  
  interface Flyable {
      // 全局常量，可以省略 public static final
      int MAX_SPEED = 7900;// 第一宇宙速度
      int MIN_SPEED = 1;
  
      // 抽象方法，可以省略 public abstract
      public abstract void fly();
  
      void stop();
  }
  
  interface Attackable {
      void attack();
  }
  
  // 全部实现接口中的方法，可以实例化
  class Plane implements Flyable {
  
      @Override
      public void fly() {
          System.out.println("飞机起飞");
      }
  
      @Override
      public void stop() {
          System.out.println("飞机降落");
      }
  }
  
  // 未全部实现接口中的方法，仍是一个抽象类
  abstract class Kite implements Flyable {
      @Override
      public void fly() {
          System.out.println("风筝在飞");
      }
  }
  
  // 实现多个接口
  class Bullet implements Flyable, Attackable {
  
      @Override
      public void fly() {
          System.out.println("子弹起飞");
      }
  
      @Override
      public void stop() {
          System.out.println("子弹停止");
      }
  
      @Override
      public void attack() {
          System.out.println("子弹具有攻击性");
      }
  }
  ```

面试题：

- 抽象类与接口有哪些异同？

  ![image-20210303100025773](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105044419-1286683543.png)

  - **接口能继承接口；**

  - **抽象类能继承接口（如不完全实现接口方法的类，还是抽象类）；**

  - **抽象类能继承非抽象类（如抽象类的父类 Object）。**

- 排错：

  - 因为接口 A 和父类 B 是并列的，所以需要明确变量 x 的所属，如果 A 是 B 的父类，那么在 C 中就近原则，x 会认为是 B 的属性：

    ```java
    interface A {
        int x = 0;
        int x1 = 2;
    }
    
    class B {
        int x = 1;
        int x2 = 3;
    }
    
    class C extends B implements A {
        public void pX() {
            System.out.println(x);// error: Reference to 'x' is ambiguous, both 'B.x' and 'A.x' match
            // System.out.println(A.x);// 0
            // System.out.println(super.x);// 1
            System.out.println(x1);// 2
            System.out.println(x2);// 3
        }
    
        public static void main(String[] args) {
            new C().pX();
        }
    }
    ```

  - 接口中的所有成员变量都默认是 public static final 的，不能在实现类中被重写：

    ```java
    interface Playable {
        void play();
    }
    
    interface Bounceable {
        void play();
    }
    
    interface Rollable extends Playable, Bounceable {
        Ball BALL = new Ball("PingPang");
    }
    
    class Ball implements Rollable {
        private String name;
    
        public String getName() {
            return name;
        }
    
        public Ball(String name) {
            this.name = name;
        }
    
        // play() 方法被认为是即重写了接口 Playable，又重写了接口 Bounceable
        @Override
        public void play() {
            BALL = new Ball("Football");// error: Cannot assign a value to final variable 'BALL'
            System.out.println(BALL.getName());
        }
    }
    ```

#### 接口匿名实现类的对象

```java
public class InterfaceTest {
    public static void main(String[] args) {
        Computer computer = new Computer();

        // 1. 创建接口的非匿名实现类的非匿名对象
        Flash flash = new Flash();
        computer.transferData(flash);

        // 2. 创建接口的非匿名实现类的匿名对象
        computer.transferData(new Printer());

        // 3. 创建接口的匿名实现类的非匿名对象
        USB phone = new USB() {

            @Override
            public void start() {
                System.out.println("手机开始工作");
            }

            @Override
            public void stop() {
                System.out.println("手机停止工作");
            }
        };
        computer.transferData(phone);

        // 4. 创建接口的匿名实现类的匿名对象
        computer.transferData(new USB() {
            @Override
            public void start() {
                System.out.println("mp3开始工作");
            }

            @Override
            public void stop() {
                System.out.println("mp3停止工作");
            }
        });
    }
}

class Computer {
    public void transferData(USB usb) {
        usb.start();
        transferDetails();
        usb.stop();
    }

    private void transferDetails() {
        System.out.println("具体传输数据的细节");
    }
}

interface USB {
    void start();

    void stop();
}

class Flash implements USB {

    @Override
    public void start() {
        System.out.println("U盘开启工作");
    }

    @Override
    public void stop() {
        System.out.println("U盘停止工作");
    }
}

class Printer implements USB {

    @Override
    public void start() {
        System.out.println("打印机开启工作");
    }

    @Override
    public void stop() {
        System.out.println("打印机停止工作");
    }
}
```

## 子类对象实例化过程

![image-20210226153054261](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105044019-557204616.png)

从结果上看：

- 子类继承父类之后，就获取了父类中声明的属性和方法。（继承性）

- 创建子类的对象，在堆空间中，就会加载所有父类中声明的属性。

  ![image-20210225171130690](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105043273-1315298851.png)

从过程上看：

- 当通过子类的构造器创建子类对象时，一定会直接或间接的调用其父类的构造器，进而调用父类的父类的构造器，直到调用了 java.lang.Object 类中空参的构造器为止。正因为加载过所有的父类的结构，所以才可以看到内存中有父类中的结构，子类对象才能够进行调用。

- 明确：虽然创建子类对象时，调用了父类的构造器，但是自始至终只创建了一个对象，即为 new 出来的子类对象。

  ![image-20210225171520474](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105042910-1317784558.png)

示例：从输出结果可以看出，在创建 Man 的实例时，先进入了父类的空参构造器，然后执行子类的空参构造器。

```java
public class Person {
    String name;
    int age;

    public Person() {
        System.out.println("父类空参构造器");
    }

    public void eat() {
        System.out.println("人吃饭");
    }

    public void walk() {
        System.out.println("人走路");
    }

    public static void main(String[] args) {
        Person person = new Man();
        person.eat();
        person.walk();
    }
}

class Man extends Person {
    boolean isSmoking;
    
    public Man() {
        System.out.println("子类空参构造器");
    }

    public void earnMoney() {
        System.out.println("男人负责挣钱养家");
    }

    @Override
    public void eat() {
        System.out.println("男人多吃肉，长肌肉");
    }

    @Override
    public void walk() {
        System.out.println("男人霸气的走路");
    }
}
输出结果：
父类空参构造器
子类空参构造器
男人多吃肉，长肌肉
男人霸气的走路
```

## 对象类型转换（casting）

![image-20210226212518453](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105042570-1654699269.png)

基本数据类型的 Casting：

- `自动类型转换`：小的数据类型可以自动转换成大的数据类型。如 long g = 20; double d = 12.0f;。
- `强制类型转换`：可以把大的数据类型强制转换（casting）成小的数据类型。如 float f = (float)12.0; int a = (int)1200L;。

对 Java 对象的强制类型转换，也称为`造型`：

- 从子类到父类的类型转换可以自动进行。
- 从父类到子类的类型转换必须通过造型（强制类型转换）实现。
- 无继承关系的引用类型间的转换是非法的。
- 在造型前可以使用 instanceof 判断。

示例：

```java
public class ConversionTest {
    public static void main(String[] args) {
        double d = 13.4;
        long l = (long) d;
        System.out.println(l);
        int in = 5;
        // boolean b = (boolean)in;
        Object obj = "Hello";
        String objStr = (String) obj;
        System.out.println(objStr);
        Object objPri = new Integer(5);
        // 下面代码运行时引发 ClassCastException 异常
        String str = (String) objPri;
    }
}
```

```java
public class Test {
    public void method(Person e) {
        // 假设 Person 类中没有 getSchool() 方法
        // System.out.pritnln(e.getSchool());// 非法，编译时错误
        if (e instanceof Student) {
            Student me = (Student) e;// 将 e 强制转换为 Student 类型
            System.out.pritnln(me.getSchool());
        }
    }

    public static void main(String[] args) {
        Test t = new Test();
        Student m = new Student();
        t.method(m);
    }
}
```

## 包装类（Wrapper）的使用

`包装类`：也叫封装类，是针对八种基本数据类型定义的相应的引用数据类型，以使得基本数据类型的变量具有类的特征。

![image-20210228120809840](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105042216-1175839617.png)

- JDK 1.5 之后，支持`自动装箱`，`自动拆箱`，但类型必须匹配。

基本类型、包装类与 String 类之间的转换：

![image-20210228125636680](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105041656-1604468251.png)

- 基本数据类型转换成包装类：
  - 装箱：基本数据类型包装成包装类的实例，通过包装类的构造器实现。例如：int i = 500; Integer t = new Integer(i);。
  - 自动装箱，例如：int i =500; Integer t = i;。
- 包装类转换成基本数据类型：
  - 拆箱：获得包装类对象中包装的基本类型变量，通过调用包装类的`.xxxValue()`方法。例如：boolean b = bObj.booleanValue();。
  - 自动拆箱，例如：Integer t = 500; int i = t;。
- 基本数据类型/包装类转换成字符串：
  - 调用字符串重载的`valueOf()`方法，例如：String fstr = String.valueOf(2.34f);。
  - 更直接的方式，连接运算，例如：String intStr = 5 + "";。
- 字符串转换成基本数据类型/包装类：
  - 通过包装类的构造器实现，例如：int i = new Integer("12");。
  - 通过包装类的`parseXxx(String s)`静态方法，例如：Float f = Float.parseFloat(“12.1”);。

面试题：

```java
public static void main(String[] args) {
    // 三目运算符比较基本数据类型，在编译阶段自动拆箱为 int 和 double 类型，由于三目运算符要求表达式 2 和表达式 3 类型一致，
    // 所以在编译阶段自动类型提升（即 int 自动类型转换为 double 类型），再自动装箱为 Object，输出时使用多态调用重写的 toString()，
    // 即 Double 包装类的 toString() 方法
    Object o1 = true ? new Integer(1) : new Double(2.0);
    System.out.println(o1);// 1.0

    Object o2;
    if (true)
        o2 = new Integer(1);
    else
        o2 = new Double(2.0);
    System.out.println(o2);// 1
}
```

```java
public static void main(String[] args) {
    Integer i = new Integer(1);
    Integer j = new Integer(1);
    System.out.println(i == j);// new了两个对象，false
    Integer m = 1;
    Integer n = 1;
    System.out.println(m == n);// 自动装箱，且在 -128 ~ 127 范围内，true
    Integer x = 128;// 相当于 new Integer(128);
    Integer y = 128;// 相当于 new Integer(128);
    System.out.println(x == y);// false
}
```

> Integer 类内部定义了 IntegerCache 结构，IntegerCache 中定义了一个 Integer[] 数组，保存了从`-128 ~ 127`范围的整数。如果使用了自动装箱的方式，给 Integer 赋值在 -128 ~ 127 范围内时，可以直接使用数组中的元素，不用 new。目的：提高效率。如果赋值超过了此范围，会 new 一个新对象。

```java
/**
 * 利用 Vector 代替数组处理：从键盘读入学生成绩（以负数代表输入结束），找出最高分，并输出学生成绩等级
 * 提示：数组一旦创建，长度就固定不变，所以在创建数组前就需要知道它的长度，而向量类 java.util.Vector 可以根据需要动态伸缩
 *  创建Vector对象：Vector v = new Vector();
 *  给向量添加元素：v.addElement(Object obj);// obj必须是对象
 *  取出向量中的元素：Object obj = v.elementAt(0);
 *  注意第一个元素的下标是 0，返回值是 Object 类型的
 *  计算向量的长度：v.size();
 *  若与最高分相差10分内：A等；20分内: B等；30分内: C等；其它: D等
 */
public class ScoreTest {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Vector<Object> objects = new Vector<>();
        int maxScore = -1;
        while (true) {
            int nextScore = scanner.nextInt();
            if (nextScore < 0) {
                break;
            }
            if (nextScore > 100) {
                continue;
            }
            objects.add(nextScore);// 自动装箱
            if (maxScore < nextScore) {
                maxScore = nextScore;
            }
        }

        char level;
        for (int i = 0; i < objects.size(); i++) {
            Object object = objects.elementAt(i);
            int score = (Integer) object;// 自动拆箱
            if (maxScore - score < 10) {
                level = 'A';
            } else if (maxScore - score < 20) {
                level = 'B';
            } else if (maxScore - score < 30) {
                level = 'C';
            } else {
                level = 'D';
            }
            System.out.println("Student-" + i + " score is " + score + ", level is " + level);
        }
    }
}
```

## Object 类的使用

`Object 类是所有 Java 类的根父类。`

如果在类的声明中未使用 extends 关键字指明其父类，则默认父类为 java.lang.Object 类。

![image-20210227162812948](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105041300-1733157896.png)

验证方法：

```java
public class ObjectTest {
    public static void main(String[] args) {
        Base base = new Base();
        System.out.println("父类：" + base.getClass().getSuperclass());// 父类：class java.lang.Object
    }
}

class Base {
}
```

Object 类中的主要结构：

![image-20210227163126263](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105040956-1354041367.png)

### == 与 equals()

**`==`**：

- 如果**比较的是基本数据类型变量**：比较两个变量保存的数据是否相等，不一定类型要相同。

  ```java
  public static void main(String[] args) {
      int i = 10;
  
      int j = 10;
      System.out.println(i == j);// true
  
      double k = 10.0;
      System.out.println(i == k);// true
  
      char c = 10;
      System.out.println(i == c);// true
  
      char c1 = 'A';
      char c2 = 65;
      System.out.println(c1 == c2);// true
  }
  ```

- 如果**比较的是引用数据类型变量**：比较两个变量的地址值是否相同，即两个引用是否指向同一个对象实体。

  ```java
  // String 类比较特殊，要注意
  public static void main(String[] args) {
      String s1 = "javacdfa";// 这样写的 javacdfa，位于常量池中
      String s2 = "javacdfa";
      System.out.println(s1 == s2);// true
  
      String s3 = new String("iam");// 这样 new 的，在堆内存中 
      String s4 = new String("iam");
      System.out.println(s3 == s4);// false
  }
  ```

- **用 == 进行比较时，符号两边的数据类型必须兼容（可自动转换的基本数据类型除外），否则编译出错。**

**`equals()`**：

- 是一个方法，而非运算符，只能适用于引用数据类型。

- 使用格式：`obj1.equals(obj2)`。

- 所有类都继承了 Object，也就获得了 equals() 方法，也可以对其重写 。

- Object 类中 equals() 方法的定义：

  ```java
  public boolean equals(Object obj) {
      return (this == obj);
  }
  ```

  - 说明：其作用与 == 相同，比较是否指向同一个对象 。

- 像 File、String、Date 及包装类等，都重写了 Object 类中的 equals() 方法，重写以后，比较的不是两个引用对象的地址是否相同，而是比较两个引用对象的 "实体内容" 是否相同。比如 String 类的 equals() 方法：

  ```java
  public boolean equals(Object anObject) {
      // 先判断地址
      if (this == anObject) {
          return true;
      }
      // 再判断内容
      if (anObject instanceof String) {
          String anotherString = (String)anObject;
          int n = value.length;
          if (n == anotherString.value.length) {
              char v1[] = value;
              char v2[] = anotherString.value;
              int i = 0;
              while (n-- != 0) {
                  if (v1[i] != v2[i])
                      return false;
                  i++;
              }
              return true;
          }
      }
      return false;
  }
  ```

- 通常情况下，自定义的类使用 equals() 方法时，也是比较两个引用对象的 "实体内容" 是否相同，那么，就应该重写 equals() 方法。

- 重写 equals() 方法的原则：

  - `对称性`：如果 x.equals(y) 返回是 true，那么 y.equals(x) 也应该返回是 true。
  - `自反性`：x.equals(x) 必须返回是 true。
  - `传递性`：如果 x.equals(y) 返回 true，而且 y.equals(z) 返回 true，那么 z.equals(x) 也应该返回 true。
  - `一致性`：如果 x.equals(y) 返回是 true，只要 x 和 y 内容一直不变，不管重复 x.equals(y) 多少次，返回都是 true。
  - 任何情况下，`x.equals(null)`永远返回 false；`x.equals(和x不同类型的对象)`永远返回 false。

**== 和 equals() 的区别：**

- == 既可以比较基本类型也可以比较引用类型。对于基本类型是比较值，对于引用类型是比较内存地址。
- equals() 方法属于 java.lang.Object 类里面的方法，如果该方法没有被重写过，默认也是 ==。
- 具体到特定自定义的类，要看该类里有没有重写 Object 的 equals() 方法以及重写的逻辑。
- 通常情况下，重写 equals() 方法，是比较类中的相应属性是否都相等。 

### toString()

当输出一个对象的引用时，实际上就是调用当前对象的`toString()`方法。

```java
public class ObjectTest {
    public static void main(String[] args) {
        Order order = new Order();
        System.out.println(order);// cn.zeloud.database.Order@78308db1
        System.out.println(order.toString());// cn.zeloud.database.Order@78308db1
    }
}

class Order {

}
```

Object 类中 toString() 方法的定义： 

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

像 File、String、Date 及包装类等，都重写了 Object 类中的 toString() 方法，使得在调用对象的 toString() 方法时，返回相应的 "实体内容"。

自定义类也可以重写 toString() 方法，当调用此方法时，返回相应的 "实体内容"。比如 String 类的 toString() 方法：

```java
public String toString() {
    return this;
}
```

- 基本类型数据转换为 String 类型时，调用了对应包装类的 toString() 方法。

面试题：

```java
public static void main(String[] args) {
    char[] arr = new char[] { 'a', 'b', 'c' };
    System.out.println(arr);// abc
    int[] arr1 = new int[] { 1, 2, 3 };
    System.out.println(arr1);// [I@78308db1
    double[] arr2 = new double[] { 1.1, 2.2, 3.3 };
    System.out.println(arr2);// [D@27c170f0
}
```

### main()

由于 Java 虚拟机需要调用类的`main()`方法，所以该方法的访问权限必须是 public，又因为 Java 虚拟机在执行 main() 方法时不必创建对象，所以该方法必须是 static 的，该方法接收一个 String 类型的数组参数，该数组中保存执行 Java 命令时传递给所运行的类的参数。

又因为 main() 方法是静态的，我们不能直接访问该类中的非静态成员，必须创建该类的一个实例对象后，才能通过这个对象去访问类中的非静态成员，这种情况，我们在之前的例子中多次碰到。

main() 方法的使用说明：

- main() 方法是`程序的入口`。
- main() 方法也是一个普通的静态方法，在执行某个类的 main() 方法之前，需要先加载这个类，这个过程是早于 main() 方法中首行的执行语句的。
- main() 方法可以作为程序与控制台交互的方式之一，其他的还可以使用 Scanner 类。

命令行参数用法举例：

![image-20210301141727952](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241001105040360-1180513405.png)

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/JavaLanguage/java.md