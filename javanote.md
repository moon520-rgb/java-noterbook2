# 一：封装

## 1:概念

  所谓封装，其实就是不想让外部随便乱改值，通常步骤是把一个属性私有化，**因为私有化了**，**所以就不能通过外部来改值了**，这时候就要在内部写两个方法，一个set，一个get，set是为这个私有化成员赋值，get是得到这个私有化成员的值，**一般在set方法里会有权限判断，比如你满足某个条件，才能修改值，否则不能修改**，这就叫做封装。

![image-20240925095713444](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925095713444.png)



## 	2:代码展示

```java
public class Main {
    public static void main(String[] args) {
        Student student = new Student();
        student.setName("jack");
        System.out.println(student.name);
        student.setScore(15);
        System.out.println(student.getScore());
    }
}

class Student{
    String name;
    private double score;

    public String  getName() {
       return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        if(score>100||score<0)
            System.out.println("你设置了了错误的成绩，请重新输入");
        else
            this.score = score;
    }
}
```

如上图，该student有两个类，一个是公开的name，一个是私有的score，<font size=5 color=red>外部成员不能通过person.score来修改和得到成绩，所以只有写一个set方法来修改，其中还有对输入成绩对错的判断(其实就是权限设置，你不满足这个权限，你就不能修改)。通过get方法来得到成绩。</font> 至于name，其实我感觉是不用set和get方法的，因为它是公开的，外部可以直接获取到。**然后这里的封装方法可以用快捷键alt加f11（insert）来快速写**。

<font size=5 color=blue>封装就是专门为private设置的</font>





# 二：继承



## 1:为什么有继承



当两个类的属性和方法大多数都相同时，可以用到继承，减少代码的重复性。

![image-20240925101727578](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925101727578.png)

把共有的属性和方法写成父类，其它类的只需要继承父类，然后写自己特有的属性和方法。





## 2:继承的步骤

![image-20240925102041145](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925102041145.png)



## 3:使用细节

### （1）：关于父类中的private属性和方法如何在子类调用

![image-20240925105010468](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925105010468.png)



```java
public class Pupil {//父类
    String name="jack";
    private int age=15;
    private void test400(){
        System.out.println("输出400");
    }

    public int getAge() {
        return age;
    }
    public void test300(){
        System.out.println("输出为:300");
    }
    private void test200(){
        System.out.println("输出200");
    }
    public void getTest200(){
        test200();
    }
}
```

如上图，首先是有个父类，有公有属性name，私有属性age，公有方法test300，私有方法test200。

<font size=5 color=red>因为私有属性和方法不能在子类访问，所以需要在父类中写调用私有属性和私有方法的公共方法，也就是如上的getAge和getTest200.</font>

如下图，公有属性和方法在父类可以直接调用，而私有属性和方法需要通过**父类的公有方法来调用**。

```java
public class Graduate extends Pupil{
    public void test()
    {
        System.out.println("父类的名字为" + name+","+"父类的年龄为:"+getAge());
        test300();//直接调用公用方法
        getTest200();//调用私有方法
    }
}
```





### （2）：子父类构造器

![image-20240925110308084](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925110308084.png)



![image-20240925110953391](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925110953391.png)

简而言之，如果父类有无参构造器，可以什么都不写，自己就调用了，如果在父类自己定义了有参构造器(会覆盖默认的无参构造器),则必须在子类的构造器中加个super（给父类构造器的参数赋值）才可以。**通过传入参数的个数指定调用父类的哪个构造器**，因为构造器的名字都一样嘛。

<font size=5 color=red>总而言之，子类怎么都要调用父类的构造器，当父类没有构造器时，默认是无参构造器，这时候子类可以什么都不写，其实就是默认super(),父类有多个构造器时，子类就要通过super(参数)，根据参数的个数去选择调用父类的哪一个构造器,例子如下</font>

![image-20240925111539888](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925111539888.png)

此时父类有三个构造器，一个无参，一个带一个参数，一个带两个参数。

### （3）:什么时候用继承

![image-20240925112804263](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925112804263.png)

### （4）：子类父类的查找关系(本质）

![image-20240925114516248](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925114516248.png)

当Son son=new Son()时，整个内存如图所示，**子父类的属性都是单独的，**当输出son.name时，会输出大头儿子，这是因为会有个查找关系，如下图

![image-20240925114744818](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925114744818.png)

**这里的可以访问，就是是公有的，如果是私有的，则直接报错，不会继续查找上一级**.方法查找也一样



## 4：练习

![image-20240925115316044](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925115316044.png)

这里会先执行B的无参构造器，然后遇到this("abc")，this就是在一个构造器中调用另一个构造器，直接传参，所以会执行B的有参构造器，<font size=5 color=red>注意，在b的有参构造器中，默认有个super(),所以再执行A的无参构造器，输出a,然后输出b name</font>,最后输出b。

![image-20240925131718464](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925131718464.png)

有this就没有默认的super()

我是A类，

hahah 我是B类有参构造，

我是c类有参构造

我是c类的无参构造

# 三:super

## 	1：基本介绍

![image-20240925150215283](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925150215283.png)

这里属性和方法不用super也能直接访问，前提是public的修饰符，但是如果后面把父类的方法重写了，就需要用到super来明确指定调用父类的方法和属性。

比如父类有个**方法**是cal(),子类想要调用，有三钟方法

1:直接调用。cal()    还是遵循那个查找原则(**在子父类的查找关系那**)，从本类开始查找，然后到父类，一直到最顶层的类。

2:用this。    this.cal()    同上

3:用super(): s    uper.cal()   **跳过本类**，直接从父类开始查找。

## 2：使用细节

![image-20240925152100492](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925152100492.png)

## 3：super和this的比较

![image-20240925152249729](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925152249729.png)





# 四：方法重写

## 1：基本介绍

![image-20240925152624021](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925152624021.png)

这时候如果调用这个方法，就是调用的子类的方法。想调用父类的，需要用super.方法名





## 2：细节

![image-20240925153112923](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925153112923.png)



## 3：重写和重载的区别

![image-20240925153542577](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240925153542577.png)

# 五：多态

## 1：方法的多态

![image-20240926101125564](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926101125564.png)

![image-20240926101153780](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926101153780.png)

如上图，在A类里，通过不同的参数个数调用sum方法，就会调用不同的sum方法，因此对于sum方法来说，就是多种状态的体现

## 2：对象的多态

![image-20240926101742141](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926101742141.png)

具体的如下图：



![image-20240926102326413](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926102326413.png)





如上，Animal是Dog和Cat的父类，然后他们三个类里都有一个cry方法。

<font size=5 color=red>Animal animal=new Dog(),因为左边是编译类型，右边是运行类型，所以编译类型就是Animal类，但运行类型是Dog类，而程序在执行的时候是执行的运行类型。所以此时animal.cry（）执行的就是Dog类型里的cry方法</font>



就像是披着羊皮的狼，羊皮就是编译类型，狼就是运行类型，表面上看过去是羊皮（编译类型）,其实是狼(运行类型)



![image-20240926103149812](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926103149812.png)（这句话就是向上转型)

​								

Animal animal=new Dog()     （Dog类是Animal类的子类）

**这里的animal就是一个引用**,变量就是引用,就是指向一个对象的地址。

<font size=5 color=red>上面这句话是核心</font>





![image-20240926104250599](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926104250599.png)

用这一个方法就可以涵盖所有的主人给动物喂食的方法了，因为**父类的引用可以指向子类的对象**,所以此时Animal animal可以指向任何一个子类对象，比如Dog dog，Cat cat这种,Food 同理。此时把Dog dog传进来，animal.getName()就跟dog.getName()一样了。



![image-20240926105350531](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926105350531.png)



<font size=5 color=red>如上图，本质上讲，其实在传入参数的时候，还是传入Dog类定义的dog和Bone类定义bone，但是由于方法里Animal类是所有动物类的父类，所以它可以指向任何传进来的动物类的对象。然后后面调用方法的时候就是用这些子类的方法。用Dog dog=new Dog()开辟了一个类型为Dog类的空间，变量dog指向这个空间(存着地址),然后把这个dog传进去这个方法，其实就是把这个Dog空间的地址赋值给animal。其实就是animal=dog，等价于Aanimal animal=new Dog(),从此他们两个(animal和dog)就指向了一个空间，也就是Dog类这个空间，所以调用的方法就是Dog类的方法，而能这么直接赋值的原因其实就是animal是Animal类型的，Animal类是Dog类的父类，所以可以这么直接赋值(父类的引用可以指向子类的对象)，如果animal是int类型的,就不能这么直接赋值。</font>



## 3：使用细节

### (1)：向上转型



![image-20240926122710499](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926122710499.png)

这里父类的引用不能调用子类中特有的方法，重写的方法可以调用(父类，子类都有的方法)。查找的时候还是从子类往父类找。

### （2）：向下转型



![image-20240926123946255](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926123946255.png)

**向下转型是解决引用类型不能调用子类中特有的方法和属性而出现的**，Cat cat=（Cat）animal;

这时候父类的引用animal就可以用Cat类中特有的方法了。**这种方法不用开辟新空间**，还是在原来的对象那个空间操作。现在就相当于有两个引用指向了同一个对象，一个是父类的引用animal，一个是cat。

### (3):属性重写问题

![image-20240926125003273](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926125003273.png)

这里属性不能看成跟方法一样，属性的值看编译类型（base，父类），调用哪个方法看运行类型(Sub,子类)。所以这里是10.

## 4：instance of

![image-20240926125548178](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926125548178.png)

是就返回True，不是就返回false。

![image-20240926154410327](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926154410327.png)

判断e是不是worker类型的,如果是的话，就把e转成worker类型，并用worker1来指向它(通常用在向下转型，父类变子类，然后用子类特有的方法)。

## 5：动态绑定机制

![image-20240926133326219](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926133326219.png)

调用一个方法时，子类没有，调用到父类了，发现父类的这个方法又调用了一个方法，而此方法子类和父类都有，此时执行**子类的方法**，而属性就是就近原则。哪里调用的就执行哪里的。

## 6：多态数组

![image-20240926135239189](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926135239189.png)

其实就是一个向上转型，Person是父类，而它的引用可以指向子类的对象，把这些对象全放在一个数组里，就是多态数组。

因为调用方法是从子类开始查找，所以这里输出persons[i].say()就会去不同的子类来输出say方法，挺好用的。



当要输出某个子类特定的方法时：

![image-20240926135911512](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926135911512.png)

 

## 7：多态参数

![image-20240926140223032](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926140223032.png)



当一个父类有多个子类来继承，然后有一个方法需要输出不同子类的相同方法时(重写方法),这时候就要用到多态参数，如下图：

![image-20240926155426200](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926155426200.png)

worker类和manager类都继承的employee类，并且他们都有相同的方法，就是getAnuual，计算每个人薪水的方法。所以需要用到多态参数employee e，因为父类的引用可以指向子类的对象，所以根据传进来参数的类型来决定调用哪个子类的方法。



而workWay属于子类特有的方法，就需要用到向下转型来输出这个方法。



manageWay同理。



主函数如下：

![image-20240926155708646](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926155708646.png)

可以看到，传进去了一个worker类型，一个manager类型，就输出对应的方法。



项目以及上传到github上了



# 六：object类里的方法

![image-20240926184127173](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926184127173.png)

## 1：equals方法

### （1）：equals和==对比

![image-20240926184251494](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926184251494.png)

在将光标放在方法上按住ctrl+b，就可以查看原码



![image-20240926190311647](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926190311647.png)

因为对于引用数据类型，是比较地址是否相等，而这里是两个对象，所以第一个为假，而equals本来也是判断地址是否相等，但是在**Integer**类里equals方法被重写了，是判断对象的值是否相等，所以第二个为真。

### （2）:练习

![image-20240926190630533](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926190630533.png)

这里Person本来没有equals方法，但由于所有的类都是继承的Object类，所以默认用的是Object类里的equals，而Object的equals方法是判断两个对象是否相等，也就是地址是否相等，所以为假。



在Person类里重写equals方法后

 ![image-20240926191558745](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926191558745.png)

就是判断这两个Person类型的对象的值是否都完全一样了，如果完全一样，返回True，有一个不一样的话，就返回False。



String类的equasl方法被重写了，是比较对象的值是否相等

![image-20240926192512967](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926192512967.png)

## 2：hashcode方法

![image-20240926224034721](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926224034721.png)



1，2，3的应用如下

![image-20240926224347173](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926224347173.png)

aa和aa2的hashcode不一样,aa和aa3的hashcode一样。



## 3：toString方法

![image-20240926224852598](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926224852598.png)

全类名就是包名＋类名。



<font size=5 color=red>关于第三点，如下：,只要输出一个对象，就会调用toString方法，如果没有重写，那么就是返回全类名@hashcode</font>

![image-20240926225245922](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20240926225245922.png)

