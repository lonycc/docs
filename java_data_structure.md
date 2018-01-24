数据data：计算机能处理的各种符号集合。

数据元素data element：表示一个事物的一组数据。

数据项data item：构成数据元素的数据。

类型type：一组值的集合。

数据类型data type：一个类型和定义在该类型上的操作集合。

抽象数据类型Abstract Data Type, ADT：一个在逻辑概念上的类型和该类型上的操作集合。

数据结构data structure：对一个数据元素集合来说，如果在数据元素之间存在一种或多种特定关系，则称为数据结构。这里的“结构”，指数据元素之间存在的关系。


## 数据的逻辑结构

基本的数据逻辑结构有3种：线性结构、树结构和图结构。

线性结构：除首尾数据元素外，每个数据元素都有前后数据元素。

树结构：除根节点外，每个数据元素只有一个前驱数据元素，可有零或若干个后继数据元素。

图结构：每个数据元素可有零个或若干个前驱数据元素和后继元素。

## 数据的存储结构

数据元素在计算机中的存储表示方式称为数据的存储结构，也称物理结构。数据的存储结构要能正确地表示出数据元素之间的逻辑关系。

顺序存储结构：把数据元素存储在一块地址连续的空间中，其特点是逻辑上相邻的数据元素在物理上也相邻，数据间的逻辑关系表现在数据元素的存储位置关系上。

指针是指向物理存储单元地址的变量。由数据元素域和指针域组成的一个整体称为一个结点node。

链式存储结构：使用指针把互相直接关联的结点连接起来，其特点是逻辑上相邻的数据元素在物理上不一定相邻，数据间的逻辑关系表现在结点的链接关系上。

## 数据的操作

对一种数据类型的数据进行某种处理称为数据的操作。数据的操作是定义在数据的逻辑结构上的，每种逻辑结构都有一个操作集合。常见操作：

访问数据元素

统计数据元素个数

更新数据元素值

插入数据元素，在数据结构中增加新的结点

删除数据元素，将指定数据元素从数据结构中删除

查找---在数据结构中查找满足一定条件的数据元素。

排序---在线性结构中数据元素个数不变情况下，将数据元素按某种指定顺序排序。

> 一个算法algorithm，就是一个有穷规则的集合，其规则确定了一个解决某以特定类型问题的操作序列。-------------D·Knuth

> 欧几里德 辗转相除法

`int gcd1(int a,int b){int k=0;do{k=a%b;a=b;b=k;}while(k!=0);return a;}`

`int gcd2(int a,int b){if(b==0){return a;}if(a<0){return gcd2(-a,b);}if(b<0){return gcd2(a,-b);return gcd2(b,a%b);}}`

## 算法分析

1、时间复杂度

算法执行时间等于所有语句执行时间的总和，是算法所处理的数据个数n的函数，表示为O(f(n))，称为算法的时间复杂度。

2、空间复杂度

算法执行时要求的额外内存空间称为算法的空间复杂度。


## Java数据类型

**基本类型**

整型----byte(8)、short(16)、int(32)、long(64)

浮点型---float(32)、double(64)

逻辑型----boolean(1)

字符型----char(16)

引用类型

类class、数组array、接口interface、字符串String

数组

`int arr[] = {1,2,3};`  //定义一维数组并赋值

`String str[] = new String[3];`  //动态一维数组，在堆中开辟了3个连续的存储空间

`String str[] = new String[]{};`  //静态一维数组，数组是空的; 也可以这样写：String[] str

## 类与对象

`[<权限修饰符>] [final|abstract] class <类名> [extends <超类名>] [implements <接口名>]`  //类声明

**类结构**

```
<类声明>{
    <成员变量>
    <成员方法>
}
```

`[<权限修饰符>] [static|final] <变量类型> <变量名>`  //成员变量

`[<权限修饰符>] <返回值类型> <方法名>([<参数列表>]) [throws <异常类>]{ <方法体> }`  //成员方法

`<类名> <对象名> = new <类名>([<参数列表>])`  //对象的声明

`<对象名>.<变量名>`

`<对象名>.<方法名>`

**实例成员与类成员**

> 类成员: 用`static`修饰的类的静态成员变量

> 实例成员: 每个对象独有的实例变量和方法, 只能通过对象来调用.

**继承与多态**

`public class <子类名> extends <超类名>`

> 对象对自身的访问, 称为`this`引用

> 每个对象对超类的访问, 称为`super`引用

> 对象操作符`instanceof`用来测试一个指定对象是否为指定类的实例

**最终类与抽象类**

`final class C1`  //最终类不能被继承

`final void m1()`  //最终方法不能被子类的方法覆盖

`abstract class C2`  //任何包含抽象方法的类必须被声明为抽象类, 抽象类的子类必须实现超类的所有抽象方法, 或者将自己也声明为抽象类

`abstract void m2()`  //抽象方法必须被子类的方法覆盖, 构造方法不能被声明为抽象的

**类的多态性**

> 一个方法有多种版本, 一次调用一个版本.

> 方法的重载`method overloading`, 参数必须不同, 返回值可以相同或不同.

> 方法的覆盖`method override`, 子类集成超类中所有可被访问的成员方法, 若子类中的方法与超类方法同名, 则会覆盖.

> 子类不能覆盖超类中声明为final或static的方法, 必须覆盖abstract方法. 覆盖时, 子类方法声明必须与超类被覆盖方法声明一样.

**接口**

> 接口是没有实现的方法和变量的集合.

```
[<修饰符>] interface <接口名>{
    方法1;
    方法2;
}

[<修饰符>] class <类名> [extends <超类名>] [implements <接口名1>, <接口名2>...]
```

**内部类**

> 在一个类的内部嵌套声明另一个类.

**接口和抽象类的区别**

> 抽象方法 `abstract void fun();` 只有声明, 没有具体实现

> 抽象类只能作为`super`类被继承, 不能直接用于创建对象. 包含抽象方法的类必须被定义为抽象类, 而抽象类不一定包含抽象方法.

`[public] abstract class ClassName{ abstract void fun(); }`

**接口 [public] interface InterfaceName{}**

> 接口中可以含有变量和方法, 接口中的变量会被隐式地指定为`public static final`变量, 方法会被隐式地指定为`public abstract`方法



**java泛型**

> 参数化类型, 所操作的数据类型被指定为一个参数.

> 泛型的类型参数只能是类类型（包括自定义类）, 不能是简单类型.

> 泛型的类型参数可以有多个, 可以使用extends语句. 泛型的参数类型还可以是通配符类型. 例如`Class<?> classType = Class.forName("java.lang.String");`

```
class Gen<T[ extends Collection]>{
    //声明一个泛型类, 限制类型持有者T只能是Collection接口的实现类
    private T ob;
    public Gen(T ob){
        this.ob = ob;
    }
    public T get(){
        return ob;
    }
    public void set(T ob){
        this.ob = ob;
    }
}
```

`Gen<String> strobj = new Gen<String>("hello");` //实例化

`Gen<ArrayList> listFoo = null;·  //这里不能用Gen<Collection>声明

`listFoo = new Gen<ArrayList>(new ArrayList());`

**多接口限制**

`<T extends SomeClass & interface1 & interface2 & interface3>`  //只能限定一个类, 但可以有多个接口

**通配符泛型**

`Gen<? extends Collection> listFoo = null;` //若无extends, 则默认是允许Object及其下的任何Java类

## 泛型方法
```
public <T> void f(T x){
    System.out.println(x.getClass().getName());
}
```

> 对于static方法而言, 是无法访问泛型类的类型参数的. 想让static具有泛型能力, 就必须使其成为泛型方法.

```
public static <T> T f(T c){
    return c;
}
```
