#   DAY1

## 数值传递机制

- 值传递

- <u>引用传递机制</u>

  数组默认是引用传递机制

  ```java
  int[] arr1={0,1,2};
  int[] arr2=arr1;
  arr2[0]=4;
  //此时arr1[0]=4;
  ```

例：数组拷贝

```java
//实现数组拷贝（内容复制）,即对arr2的改动不会影响arr1
int[] arr1={0,1,2};
int[] arr2=new int[arr1.length];

for(int i=0;i<arr1.length;i++){
    arr2[i]=arr1[i];
}
```

例：数组扩容

```java
//实际上是数组拷贝
int arr={0,1,2};
int arrNew=new int[arr.length+1];

for(int i=0;i<arr.length;i++){
    arrNew[i]=arr[i];
}
arrNew[arrNew.length-1]=3;
arr=arrNew;
//实现从{0,1,2}到{0,1,2,3}的扩容
```



# DAY2

## 继承

### 继承使用细节：

-  子类继承了父类==所有==的==属性与方法==，但父类的私有属性和私有方法不能直接在子类中访问，必须通过公共的方法进行访问
-  **子类一定会先调用父类的构造器，完成父类的初始化**
-  一般情况下，无论子类使用哪个构造器，都会默认调用父类的无参构造器。如果父类没有提供无参构造器，则必须用super()指定调用父类的某一构造器，否则就会报错。
-  super（）和this（）在使用时都只能==放在第一行==，因此这两个不能在同一个构造器中出现
-  **父类构造器的调用不限于直接父类。**将直接往上追溯到Object类（顶级父类）

### 继承时属性的关系：

示例代码：

```java
public class Grandpa{
    String name="Grandpa";
    int age=66;
}

public class Father extends Grandpa{
    String name="father";
    String hoppy="ball";
}

public class Son extends Father{
    String name="son";
}

public static void main(String[] args){
    Son son=new Son();
    System.out.println(son.name);
    System.out.println(son.age);
    System.out.println(son.hoppy);
    
    //要按照查找关系来返回信息
    //(1)首先在子类中查找属性，若有且可以访问，则返回该信息
    //(2)若子类中没有，则在父类中查找该属性，若有，则返回该信息
    //(3)若父类中没有，则继续向上寻找上级父类，直到Object
}

//输出：son 66 ball
```

#### ==当继承发生时的内存布局==

![QQ图片20220206160750](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220206160750.png)



### super关键字：

- 访问父类的属性，但不能访问父类的私有属性：super.属性
- 访问父类的方法，但不能访问父类的私有方法：super.方法
- ==super不仅可以用来访问直接父类==，也可以用来访问爷爷类的属性和方法。若多个上级父类中都有相同的成员，则依据==就近原则==

### super与this的比较

![QQ图片20220206195839](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220206195839.png)

## 多态

### ==动态绑定机制==：

- **当调用对象的==非静态方法==时，该方法会和对象的==内存地址/运行类型==绑定**
- **当调用对象==属性==时，不会有动态绑定机制**

```java
例：判断对错。在java的多态调用中，new的是哪一个类就是调用的哪个类的方法。（错）
 //链接：https://www.nowcoder.com/questionTerminal/e5556245c9cc467885c9cf2dbc305f5c

public class Father {
    public void say(){
        System.out.println("father");
    }
    public static void action(){
        System.out.println("爸爸打儿子！");
    }
}
public class Son extends Father{
    public void say() {
        System.out.println("son");
    }
   public static void action(){
        System.out.println("打打！");
    }
    public static void main(String
[] args) {
        Father f=new Son();
        f.say();
        f.action();
    }
}
输出：son
           爸爸打儿子！
//当调用say方法执行的是Son的方法，也就是重写的say方法
//而当调用action方法时，执行的是father的方法。
```

- "="左边称为==编译类型==，右边称为==运行类型==
- 普通方法，运用的是==动态单分配==，是根据new的类型确定对象，从而确定调用的方法；
- 静态方法，运用的是==静态多分配==，即根据静态类型确定对象，因此不是根据new的类型确定调用的方法
- **对于成员变量，看左边；对于非静态成员函数，看右边；对于静态成员函数，看左边。**

### 多态的属性的细节：

-  属性没有重写一说，属性的值看==编译==类型

  ```java
  class AA{
   int value=10;
  }
  class BB extends AA{
      int value=20;
  }
  
  public static void main(String[] args){
      AA aa=new BB();
      System.out.println(aa.value);
  }
  
  //输出结果：10
  ```

-  **instanceof，用于判断对象的==运行类型==是否XX类型或XX类型的子类型**

   

   

### 向上转型

- 向上转型：父类的引用指向了子类的对象

- 语法：父类类型  变量名=new 子类类型（）；

  ```java
  Animal animal=new Cat();
  ```

  ### 特点：编译类型看左边，运行类型看右边

  - **可以调用父类的所有成员（需遵循访问权限）**
  - **不能调用子类的特有成员**



### 向下转型

语法：子类类型 引用名=（子类类型）父类引用

**细节**：

- 只能强转父类的引用，不能强转父类的对象
- 要求父类的引用必须指向的是当前目标类型的对象
- 当向下转型后就可以调用子类的所有成员





## 自动转换类型与自动封装

```java
int a='a';//char类型自动转化为int
double b=3;//int类型自动转化为double
Double c=3.0;//会自动封装
Double d=3；//会编译错误
```

## 强制转化

- 高精度转化为低精度数据,会造成精度丧失

```java
int a=(int)1.6;
char b=(char);
```

# DAY3

## 迷宫问题：老鼠走迷宫

```java
public class Maze {

    //0是可以走，1是障碍物，2是可以走到，3是走过，但走不通，是死路
    //下——右——上——左
    private int[][] map;
    boolean sign;

    public void initMap(){

        sign=false;
        map=new int[8][7];
        for(int i=0;i<8;i++){
            for(int j=0;j<7;j++){
                if(i==0||i==7){
                    map[i][j]=1;
                }else{
                    if(j==0||j==6){
                        map[i][j]=1;
                    }
                }
            }
        }
        map[5][1]=1;
        map[5][2]=1;
    }

    public void print(){
        System.out.println("====打印迷宫====");
        for(int i=0;i<8;i++){
            for(int j=0;j<7;j++){
                System.out.print(map[i][j]);
            }
            System.out.println();
        }
    }

    
    //自己写的findWay()
    //还写得有问题
    //什么垃圾玩意
    public boolean findWay(int x,int y){
        if(map[6][5]==2){
            sign=true;
            return true;
        }
        else if(map[y][x]==1){
            System.out.println("Out of border");
            return false;
        }
        else {
            if (x < 1 || x > 5 || y < 1 || y > 6) {
                System.out.println("Out of border");
                return false;
            } else {
                map[y][x] = 2;
                if (y > 0 && y < 5 && map[y + 1][x] == 0&&!sign) {
                    findWay(x, y + 1);
                }
                if (x > 0 && x < 5 && map[y][x + 1] == 0&&!sign) {
                    findWay(x + 1, y);
                }
                if (y > 1 && y < 7 && map[y - 1][x] == 0&&!sign) {
                    findWay(x, y - 1);
                }
                if (x > 1 && x < 6 && map[y][x - 1] == 0&&!sign) {
                    findWay(x-1,y);
                }
            }
        }
        if(!sign){
            map[y][x]=3;
            return false;
        }
        return true;
    }
    
    
    //真正的findWay()
    public boolean findWay(int row,int col){
        if(map[6][5]==2){
            return true;
        }else{
            if(map[row][col]==0){
                map[row][col]=2;
                if(findWay(row+1,col)){
                    return true;
                }else if(findWay(row,col+1)){
                    return true;
                }else if(findWay(row-1,col)){
                    return true;
                }else if(findWay(row,col-1)){
                    return true;
                }else{
                    map[row][col]=3;
                    return false;
                }
            }else{
                return false;
            }
        }
    }

    
    
    public static void main(String[] args){
        Maze maze=new Maze();
        maze.initMap();
        maze.print();
        if(maze.findWay(1,1)){
            System.out.println("Finded the way");
            Maze.print();
        }else{
            System.out.println("Not finded the way");
        }
    }
}
```

# DAY4

## 八皇后问题

在一个8*8的棋盘上放八个棋子，要求任意两个棋子不能在同一行或同一水平线或同一斜线上

```java
package PAPER;

public class EightQueen {

    int[] chessBoard;
    int soluations;//记录摆放方案

    public void initBoard(){
        chessBoard=new int[8];
        soluations=0;
    }

    public boolean solve(int row){
        if(row==8){
            soluations+=1;
            return true;
        }else{
            for(int col=0;col<8;col++){
                if(isLegal(chessBoard,row,col)){
                    solve(row+1);
                }
            }
        }
        return false;
    }

    //检测前row+1行是否合法
    public boolean isLegal(int[] board,int row,int col){
        board[row]=col;
        int[] check=new int[8];
        check[0]=board[0];
        for(int i=1;i<=row;i++){
            if(isRight(board[i],check,i)){
                check[i]=board[i];
            }else{
                board[row]=0;
                return false;
            }
        }
        return true;
    }

    public boolean isRight(int num,int[] arr,int row){
        for(int i=0;i<row;i++){
            if(num==arr[i]){
                return false;
            }
            if((row-i)==(num-arr[i])||(row-i+num-arr[i]==0)){
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args){
        EightQueen Queen=new EightQueen();
        Queen.initBoard();
        Queen.solve(0);
        System.out.println("有"+Queen.soluations+"种解法");
    }
}
```



## 可变参数使用

基本语法

```java
//访问修饰符 返回类型 方法名(数据类型... 形参名){...}，表示可以接收0-N个某种数据类型的参数
```

用例

```java
//计算N个数的和
//此时nums可以当做数组处理，即nums就是int型数组
public int sum(int... nums){
    int sum=0;
    for(int i=0;i<nums.length;i++){
        sum+=nums[i];
    }
    return sum;
}
```

- **使用细节**

  - 可变参数的本质是数组

  - 可变参数的实参可以是数组

    ```java
    public class T{
        public static void main(String[] args){
            T t=new T();
            int[] arr=new int[9];
            t.f1(arr);//直接传数组进去
        }
        public int f1(int...nums){...}
    }
    ```

  - 当可变类型参数和普通参数一起使用时，可变类型参数必须放在参数列表末尾



# DAY5

## IDEA快捷方式

- 格式化代码：ctrl+alt+L

- 生成构造器：alt+insert->constructor

- 得到IDEA提供的重写后的toString用于输出属性：alt+insert->toString

- 将光标放到一个方法上，ctrl+b，可以快速定位到该方法所在的类

- 快速创建变量名：new 类名().var，IDEA就会自动分配一个变量名

- 查看源码:将光标放在方法上，ctrl+b

- 快速生成循环体：

  ```java
  //循环次数.for->回车
  //即会生成循环体：
  for(int i=0;i<循环次数;i++){
      ...
  }
  ```

  

# DAY6

## 访问修饰符：

- 公开级别：==public==，对外公开
- 受保护级别：==protected==，对子类和同一个包中的类公开
- 默认级别：没有修饰符，向同一个包中的类公开
- 私有级别：==private==，只有类本身可以访问，不对外公开

### 使用细节：

- 只有==默认==和==public==可以用来修饰类

  

# DAY7

## Object类详解

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220207141725.png)

### equals

- equals只能用来比较引用类型
- 是Object类中的方法，默认只能比较地址是否相同。在子类中往往==重写==，用于比较内容是否相同，比如Integer和String

- equals与“==”的比较：

  1. “==”既可以比较基础类型，也可以比较引用类型

  2. “==”用于比较基础类型时，比较的是值是否相同

  3. “==”用于比较引用类型时，比较的是**地址是否相同**，即**是否是同一个对象**

     ```java
     public static void main(String[] args){
         A a=new A();
         A b=a;
         A c=b;
         System.out.println(a==b);//true
         System.out.println(a==c);//true
         System.out.println(b==c);//true
         B obj=new A();
         System.out.println(obj==a);//true
     }
     
     class B{}
     class A extends B{}
     ```

     

### hashCode方法

- 如果指向的是同一个对象，则哈希值一定相同
- 如果指向的不是同一个对象，则一般哈希值不同
- 哈希值主要是根据地址号来的，但不能完全等同于地址

### toString方法

- 默认返回：==全类名（包名+类名）==+@+哈希值的十六进制，子类往往重写toString方法

- 重写toString方法，打印对象或拼接对象时，都会自动调用对象的toString方法

- 当直接输出一个对象时，会默认调用toString方法

- ```java
  //源码
  public String toString(){
      return getClass().getName()+"@"+Integer.toHextString(hashCode());
  }
  ```

- IDEA提供了一个重写后的toString用于输出属性：快捷键alt+insert->toString

### finalize方法

- 当对象被回收时，会自动调用该对象的finalize方法。可以自己重写该方法
- 什么时候对象会被回收：当没有引用指向该对象时，该对象就会被回收
- 垃圾回收机制是由系统的GC算法调用的，即回收对象并不是即时的。我们也可以通过==System.gc()==来主动调用GC算法实现即时回收



# DAY8

## 断点调试

### 断点调试快捷键

- F7：跳入方法内
- F8：逐行执行代码
- shift+F8：跳出方法
- F9：执行到下一个断点
- ![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220208140225.png)

# DAY9

## main方法

- **理解main方法的形式：**

  public static void main(String[] args){}

  1. main方法由jvm调用，因此访问权限必须是public
  
  2. jvm在调用main方法时不创建对象，因此必须是static
  
  3. 该方法接受String类型的数组参数，该数组中保存执行java命令时传递给所运行类的参数
  
     ```java
      public static void main(String[] args){
             EightQueen Queen=new EightQueen();
             Queen.initBoard();
             Queen.solve(0);
             System.out.println("有"+Queen.soluations+"种解法");
             for(int i=0;i< args.length;i++){
                 System.out.println(args[i]);
             }
         }
     ```
  
     ![image-20220210094909732](C:\Users\38156\AppData\Roaming\Typora\typora-user-images\image-20220210094909732.png)
  
     

- **注意**：
  1. **main方法是静态方法，所以如果想访问该类中非静态成员，只有通过创建该类的对象，通过该对象去访问非静态成员**

- **IDEA中向main方法传参数**

  

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220210163401.png)

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220210163446.png)

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220210163535.png)

- **控制台传入参数**：

  java eightqueen.java 参数1 参数2 参数3



## 代码块

- **基本语法：**

   

  ```java
  [修饰符] {
  ...
  }
  ```

  - 修饰符可选，若选也只能是==static==

- **对代码块的理解：**

  1. 作为初始化的另一种方法
  2. 代码块在==创建对象==或==加载类==时被隐式调用
  3. 运行时代码块先于构造器运行
  4. 若多种构造器都有相同的步骤，则可以将其写入代码块中，使代码简洁

- **代码块使用细节**

  1. static代码块又称静态代码块，随==类的加载==而运行，而且==只会运行一次==。若是普通代码块，则每创建一个对象就会运行一次
  2. static属性也会随==类的加载==而运行
  3. **什么时候类会被加载：**
     1. 对象被创建时
     2. 子类被创建时，父类也会被加载，而且父类先加载，子类后加载
     3. 使用类的静态成员时
  4. 若只是调用类的静态成员，则==普通代码块不会被调用==

- **创建对象时，在一个类中的调用顺序是==（重要）==：**

  1. 静态代码块和静态成员属性（两者按定义顺序，下同）
  2. 普通代码块和普通成员属性
  3. 调用构造器
  4. **构造器的最前面其实隐含了super（）和==调用普通代码块==**

- **创建一个子类时，静态代码块、静态属性、普通代码块、普通属性、构造方法的顺序**==（难点）==

  1. 父类的静态代码块和静态属性

  2. 子类的静态代码块和静态属性

  3. 父类的普通代码块和普通成员属性

  4. 父类的构造函数

  5. 子类的普通代码块和普通成员属性

  6. 子类的构造函数






# DAY10

## 设计模式

- **单例设计模式：**

  - **介绍**：单例设计模式就是，在整个软件中保证只存在一个对象实例，且该类只提供一个取得该对象实例的方法

  - **设计思路**：

    1. **饿汉式**：

       - **设计步骤**：

         1. 该类构造器私有化

         2. 该类内部创建一个该类的实例对象(权限为私有且须静态)

         3. 向外暴露一个公共的取得该实例对象的静态方法

         4. 代码实现：

            ```java
            //只能有一个GirlFriend
            public Person{
                public static void main(String[] args){
                    GirlFriend girlFriend=GirlFriend.getInstance();
                }
            }
            
            class GirlFriend{
                private String name;
                private static GirlFriend girlFriend=new GirlFriend("zjm");
                
                private GirlFriend(String name){
                    this.name=name;
                }
                
                public static GirlFriend getInstance(){
                    return girlFriend;
                }
            }
            ```

            

    2. **懒汉式：**
    
       - **设计步骤**：
    
         1. 构造器私有化
    
         2. ==定义==一个private的static对象
    
         3. 提供一个public的静态方法返回该类对象
    
            ```java
            public Person{
                public static void main(String[] args){
                    GirlFriend girlFriend=GirlFriend.getInstance();
                }
            }
            
            class GirlFriend{
                private String name;
                private static GirlFriend girlFriend;
                
                private GirlFriend(String name){
                    this.name=name;
                }
                
                public static getInstance(String name){
                    if(girlFriend==null){
                        girlFriend=new GirlFriend(name);
                    }
                    return girlFriend;
                }
            }
            ```
    
    3. **饿汉式和懒汉式的主要区别：**
    
       - 饿汉式在类加载时就被创建，懒汉式则是在使用时才被创建
       - 饿汉式会浪费资源，因为有时没有用到对象但对象却被创建了
       - 懒汉式会有**线程安全问题**

# DAY11

## 接口

- ### 使用语法：


```java
interface jiekouming{
    //属性
    //方法（抽象方法、默认方法、静态方法）
}
class leiming implements jiekouming{
    //自己属性
    //自己方法
    //必须实现的接口的抽象方法
}
```

******JDK8.0之后接口中可以有静态方法、默认方法（==用default修饰==）**

- **接口使用细节**

1. **接口不可以被实例化对象**
2. **接口中所有的方法都是public，且abstract修饰符可以被省略**
3. **抽象类实现接口可以不实现接口的方法**
4. **接口中的属性必须是==final==且必须是==public static final==（一定是==静态且必须初始化==）**
5. **接口中属性的访问形式：接口名.属性名**

- **接口的多态特性**

  - **接口的多态**

    1. 虽然接口不能被创建实例对象，但接口引用可以指向一个实现了该接口的类

       ```java
       class AA implements IA{}
       interface IA{}
       public static void main(String[] args){
           IA ia=new AA();
       }
       ```

    2. 当方法形参是一个接口类型参数时，可以传入一个实现了该接口的类实参

    3. 接口类型数组可以存放实现了该接口的对象

  - **接口的多态传递**

    **若A接口继承了B接口，而类实现了A接口，则认为类也实现了B接口**

    

# DAY12

## 内部类

- **内部类可分为四类**：
  - 定义在外部类的局部位置(如方法内)
    1. 局部内部类（有类名）
    2. 匿名内部类
  - 定义在外部类的成员位置上
    1. 成员内部类（没有**static**修饰）
    2. 静态内部类（有**static**修饰）

- ### **局部内部类**
  
  1. 可以直接访问外部类的所有成员，包括私有成员
  2. 不可以添加修饰符。因为其本质是局部变量，局部变量是不能有修饰符的，但可以用**final**修饰，因为**final**本身就可以修饰局部变量
  3. 作用域：仅在定义他的方法体或代码块中
  4. 当外部类的成员和局部内部类的成员重名时，遵循就近原则。如果想访问外部类成员，使用：**外部类名.this.成员名**去访问（**外部类名.this**的本质就是**外部类对象**）
  5. 
  
- **匿名内部类**==**（重要！！！）**==

  - **基础说明**：

    1. 本质是一个类
    2. 同时还是一个对象
    3. **匿名内部类实际是有名字的**，由系统分配，命名方法：外部类名$1;

  - **语法**：new 类名/接口名（参数列表）{...}；

  - **实例解读（基于接口）**

    ```java
    //需求：使用IA接口并创建对象
    //传统方案：写一个类，实现该接口，并创建对象
    //理想方案：该类只使用一次，以后就不再使用了
    //此时使用匿名内部类来简化开发
    interface IA{
        public void cry();
    }
    public class Outer{
        private int num;
        
        public void method(){
            IA iA=new IA(){
                public void cry(){
                    System.out.println("www");
                }
            };//JDK在底层立即创建Outer$1类（实现了IA接口），并创建实例返回给iA
        } 
    }
    ```

    - **实际是JDK在底层立即创建Outer$1类（实现了IA接口），并创建实例返回给iA**
    - 此时iA的==编译类型==:IA
    - 此时iA的==运行类型==：就是匿名内部类，即外部类名$1
  
  - **实例解读（基于类）**
  
    ```java
    public Outer{
       
        public void method(){
           Person person=new Person("michael"){//参数会传递给Person的构造器
            ...  
           };
        }
    }
    class Person{
        String name;
        public Person(String name){
            this.name=name;
        }
    }
    //person编译类型：Person
    //person运行类型：
    ```
    
    - **实际是JDK在底层立即创建Outer$2类（==继承了Person==），并创建实例返回给person**
    - 编译类型：Person
    - 运行类型：Outer$2
  
- ### 成员内部类

- ### 静态内部类

  - **使用**：

    1. 可以访问外部类的所有的静态成员，但不能访问非静态的成员

  - **外部类访问静态类：**先创建对象再访问

  - **外部其他类访问静态类**：

    ```java
    Outer.Inner inner=new Outer.Inner();
    ```

    

# DAY13

## 枚举

- ### 自定义枚举类

  1. **构造器私有化**，防止直接创建
  2. 成员属性用**final static**修饰防止被更改
  3. 枚举对象名通常全部大写

  ```java
  class Season{
      private String season;
      private String desc;
      
      private static final Season SPRING=new Season("春天","温暖");
      ...
      
      private Season(String season,String desc){
          this.season=season;
          this.desc=desc;
      }
  }
  ```

- **使用enum关键字来实现枚举类**

  1. 用enum替换class

  2. 用SPRING("春天","温暖")带替换 private static final Season SPRING=new Season("春天","温暖");

  3. 常量间用逗号分隔

  4. 常量必须必须写在最前面

     ```java
     enum Season{
         private String season;
         private String desc;
         
         SPRING("春天","温暖"),SUMMER("夏天","炎热");
         
         private Season(String season,String desc){
             this.season=season;
             this.desc=desc;
         }
     }
     ```

  5. ```java
     enum Gender{
         BOY,GIRL;
     }
     ```

     上面的代码是没有错误的，**相当于调用了默认的无参构造器**

## 注解（Annotation）

### 基本的注解介绍：

- **@Override**:限定某个方法，是重写父类方法。只能用于修饰方法
- **@Deprecated**：用于表示某个程序元素（类、方法、字段、包、参数）已经过时了（但仍可以用)
- **@SuppressWarnings**：抑制编译器警告 





# DAY14

## 包装类

- 

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220214221134.png)

- **自动装箱与自动拆箱**

  ==**实质是JDK在底层调用valueOf()和inValue()方法**==

  ```java
  int n1=100;
  Integer integer=n1;//JDK在底层调用valueOf()
  
  int n2=integer;//JDK在底层调用intValue()
  ```

  

  - ### **valueOf()的底层源码**

  ```java
  public static Integer valueOf(int i) {
          if (i >= IntegerCache.low && i <= IntegerCache.high)//即-128-127之间
              return IntegerCache.cache[i + (-IntegerCache.low)];
          return new Integer(i);
      }
  ```

  **==可见，当i处于-127-128之间时，valueOf并不会返回Integer对象，而是直接返回值==**

  **->**

  **==面试题==**

  ```java
  public void method(){
      Integer n=new Integer(1);
      Integer m=new Integer(1);
      System.out.pirntln(n==m);
      
      Integer a=1;
      Integer b=1;
      System.out.println(a==b);
      
      Integer x=128;
      Integer y=128;
      System.out.println(x==y);
      
      int i=128;
      Integer j=128;
      System.out.println(i==j);//只要有基本数据类型，则仅判断值是否相等
  }
  
  //输出结果：false true false true
  ```






## String类

- **String是final类，不可以被继承**

- **String有属性==private final char[] value==，用于存放字符串内容**

- **final修饰的value字符数组，表示value的地址不可以被修改，即下面的代码是会报错的**

  ```java
  private final char[] value={"s","e"};
  char[] ch={"t","w"};
  value=ch;//wrong
  ```

- **String的常用方法**  

  

### String的两种创建方式的区别

- **方式一：String s="abc";**

  **方式二：String s2=new String("abc");**

  - **方式一：在常量池中寻找是否有“abc”数据空间，有则直接指向，没有则在常量池中创建后再指向，最终指向的是常量池中的地址**

  - **方式二：先在堆中创建了空间，里面维护了value属性。value指向常量池中的“abc”空间，如果常量池中没有“abc”空间，则创建后再指向。最终s2指向的是堆中的空间地址**

    ![image-20220222191150515](C:\Users\38156\AppData\Roaming\Typora\typora-user-images\image-20220222191150515.png)

- ### ==练习题==

  ```java
  String str="abc"+"hjn";
  //上述创建了几个对象？
  //一个：abchjn
  ```

  **解读：对于两个==字符串常量==的相加，编译器会做出优化，直接在常量池中创建一个“abchjn”对象**

  

  

  ```java
  String s1="abc";
  String s2="hjn";
  String s3=s1+s3;
  //上述创建了一个对象？
  //三个，"abc"、"hjn"在常量池中，s3指向堆中创建的value[],value[]指向常量池中的"abchjn"
  ```

  **解读：String s3=s1+s2在底层实际等于String s3=new String("abchjn"),可以通过debug验证**





## 浮点数类型

![image-20220222182541375](C:\Users\38156\AppData\Roaming\Typora\typora-user-images\image-20220222182541375.png)

- **一般更常使用double型因为精度更高**
- **如果使用float型变量储存小数点后有多位数的小数，则输出时会丢失一部分尾数的精度**
- **定义是float类型的值可以赋给double，反之不可**
- **定义float类型的常量要在数字末尾加f**

### 使用浮点数细节

- **浮点数经计算后不能用于比较，下例：**

  ```java
  double num1=2.7;
  double num2=8.1/3;
  if(num1==num2){
      System.out.println("相等");
  }
  
  //结果是没有输出任何东西
  ```

  **因为计算机的计算是以二进制进行，而某些数值并不能用二进制精确的表示，比如0.1，这时计算就会产生精度误差，导致8.1/3在计算机中并不等于2.7**



## 大数处理方案

**当需要使用的数字过大超过long或需要保存的精度过高时，可以用BigInteger或BIgDecimal来处理**

- ## BigInteger

```java
BigInteger bigInteger=new BigInteger("99999999999999999999999999999999");
```

**当需要对BigInteger进行加减乘除时，不能直接用+、-、*、/，而是要使用对应的方法**

- ## BigDecimal

  ```java
  BigDecimal bigDeciaml=new BigDecimal("34.111111111111112343333333333333");
  ```

  **同样也不能直接进行加减乘除，而是使用对应的方法**

  ==注意==：**当使用除法divide（）时，若计算结果是一个无限循环数，则会报错。此时需要指定结果的精度**

  ```java
  BigDecimal bigDeciaml=new BigDecimal("34.1111111111111123433333333333338");
  BigDecimal bigDecimal2=new BigDecimal("3");
  BigDecimal bigDecimal3=bigDecimal.divide(bigDecimal,BigDecimal.ROUND_CEILING);
  ```

  **这样可以使结果的精度与分子一致**

# **DAY15**

## 异常

- ### 异常的分类：

  - **Error**：JVM虚拟机无法解决的严重问题，如**内存资源耗尽**

  - **Exception**：其他因编程错误或外在因素导致的一般性问题，可以使用针对性代码进行处理，如**空指针访问**，**试图读取不存在的文件**

  - **Exception**又可以分为两大类：**运行时异常**和**编译时异常**

    ![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220219205035.png)

- ### **异常体系图**

  ![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220219205055.png)

- **异常小结：**

  - **运行时异常（RuntimeException），是编译器不要求强制处理的异常（其实是编译器检测不出来），一般指编程时的逻辑错误，程序员应尽量避免。**
  - **对于运行时异常可以不做处理，因为这类异常很常见，若都做处理则会对程序的可读性和运行效率产生影响**
  - **编译时异常是编译器要求必须处理的异常**

- **try/catch/finally**

  ==练习题==

  ```java
  public class Son extends father {
  
      public static int method() {
          int i = 1;
          try {
              i++;
              String[] strs = new String[3];
              if (strs[0].equals("tom")) {
                  System.out.println(strs[0]);
              }
          } catch (NullPointerException e) {
              return ++i;
          } finally {
              ++i;
              System.out.println("i="+i);
          }
          return 0;
      }
  
      public static void main(String[] args) {
          System.out.println(method());
      }
  }
  
  //输出结果：i=4 3
  ```
  
  **程序执行到catch中的return时，因为必须执行finally中的语句，所以此时底层会用一个==temp临时变量==来保存当前i的值，即3。随后i进入到finally中变成4，但最后return的==是temp保存的值==，即3**



## throws处理机制

- **异常处理机制有两种：**

1. **try/catch/finally**：程序员在代码中捕获发生的异常，自己设计代码处理
2. **throws**：将异常抛出，交给调用者（方法）来处理，最顶级的处理者是JVM

- **JVM的处理方式：打印异常信息，并终止程序**
- **如果程序员没有选择任意一种处理方式，则程序会默认选择throws Exception**
- **throws使用细节**：
  - **子类重写父类方法，对抛出异常的要求：子类中方法抛出的异常要么与父类一致，要么是父类方法抛出异常的子类**



## 自定义异常类

- ### 步骤

  - **自定义一个异常类继承Exception或RuntimeException**
  - **继承Exception则是编译时异常**
  - **继承RuntimeException则是运行时异常（一般继承RuntimeException）**
  - **因为main默认抛出的是RuntimeException，如果继承Exception，则需要在main声明抛出Exception，即写上throws Exception**

```java
//要求，若接受的年龄不在18-100之间，则抛出一个自定义的异常
public class Test
    public static void main(String[] args){
        int age=80;
        if(age<18||age>120){
            throw new AgeException("age out of the limit");
        }
    }
}

class AgeException extends RuntimeExcepption{
    public AgeException(String message){
        super(message);
    }
}
```



#### 关于自定义异常类的继承问题

**在接口开发的过程中，为了程序的健壮性，经常要考虑到代码执行的异常，并给前端一个友好的展示，这里就得用到自定义异常，继承RuntimeException类。那么这个RuntimeException和普通的Exception有什么区别呢。**

　　**1、Exception： 非运行时异常，在项目运行之前必须处理掉。一般由程序员try catch 掉。**

　　**2、RuntimeException，运行时异常，在项目运行之后出错则直接中止运行，异常由JVM虚拟机处理。**

　　**在接口的逻辑判断出现异常时，可能会影响后面代码。或者说绝对不容忍（允许）该代码块出错，那么我们就用RuntimeException，但是我们又不能因为系统挂掉，只在后台抛出异常而不给前端返回友好的提示吧，至少给前端返回出现异常的原因。因此接口的自定义异常作用就体现出来了。**

　　**由于项目中自定义异常一般继承 RuntimeException 所以想了解一下为什么。**

**一、异常基础知识**
　　**关于异常基础知识详见之前这篇博客：Java异常处理基础知识笔记：异常处理机制、异常继承关系、捕获异常、抛出异常、异常的传播、异常调用栈、自定义异常、第三方日志库**

**二、runtimeException和exception的两种异常的用途**
　　**网上摘得一段话，比喻的很恰当：**

　　**继承Exception还是继承RuntimeException是由异常本身的特点决定的，而不是由是否是自定义的异常决定的。**

　　**例如我要写一个java api，这个api中会调用一个极其操蛋的远端服务，这个远端服务经常超时和不可用。所以我决定以抛出自定义异常的形式向所有调用这个api的开发人员周知这一操蛋的现实，让他们在调用这个api时务必考虑到远端服务不可用时应该执行的补偿逻辑（比如尝试调用另一个api）。此时自定义的异常类就应继承Exception，这样其他开发人员在调用这个api时就会收到编译器大大的红色报错：【你没处理这个异常！】，强迫他们处理。**

　　**又如，我要写另一个api，这个api会访问一个非常非常稳定的远端服务，除非有人把远端服务的机房炸了，否则这个服务不会出现不可用的情况。而且即便万一这种情况发生了，api的调用者除了记录和提示错误之外也没有别的事情好做。但出于某种不可描述的蛋疼原因，我还是决定要定义一个异常对象描述“机房被炸”这一情况，那么此时定义的异常类就应继承RuntimeException，因为我的api的调用者们没必要了解这一细微的细节，把这一异常交给统一的异常处理层去处理就好了。**

　　**总结一下：**

　　**抛出 RuntimeException（运行期才可以发现的异常），调用方法的程序员不需要知道会出这个异常。**

　　**抛出Exception的方法，调用者需要明确知道这个方法里会出现什么异常，并提示调用者要去处理这个可能得异常。**

　　**简单的说，非RuntimeException必要自己写catch块处理掉。RuntimeException不用try catch捕捉将会导致程序运行中断，若用则不会中断。**

# DAY16

## 集合


- **集合类的体系图图（熟记下图内容）**

  1. **单列集合体系图**

     ![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220226170946.png)

  2. **双列集合体系图**

     ![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220228102357.png)

- **集合体系分为两类（单列集合，双列集合）**
  
  1. **单列集合中存放的都是单个对象**
  2. **双列集合中存放的都是==K-V式==的对象**



- ## Iterator迭代器（用于遍历元素）

  **所有实现了Collection的集合类都有一个iterator方法，可以返回一个Iterator容器**

  **Iterator只是遍历元素所用的，本身并不存放对象**

- **Iterator执行原理**

  ```java
  Collection col=new ArrayList();
  
  Iterator ite=col.iterator();
  while(ite.hasNext()){//判断是否有下一个元素
      Object obj=ite.next();//返回下一个元素
      System.out.println("obj="+obj);
  }
  ```

  1. **在使用next（）方法之前必须先用hasNext（）判断，否则就会抛出一个NoSuchElementExcepiton**
  2. **如果需要再次遍历，则需要重置迭代器，重置语句：Iterator ite=col.iterator();**
  



- ### **List常用方法**


![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220227155006.png)





- ### ArrayList底层源码分析

  1. **ArrayList中维护了一个Object类型的数组elementData：transient Object elementData**
  2. **当创建ArrayList对象时，如果是无参构造器，则初始容量为0，第一次添加则容量扩充为10，以后每次扩充为之前的1.5倍**

- ### **Vector底层源码分析**
  
  1. **Vector底层也是一个对象数组:protected Object[] elementData**;
  2. **Vectors是线程同步的，即线程安全，Vector类的操作方法带有synchronized关键字**

- ### **LinkedList底层源码分析**

  - **LinkedList的底层是一个双向链表**

  - **有成员变量first、last分别指向链表的头结点和尾节点**

  - **添加元素的源码分析**

    ```java
    void linkLast(E e){
        final Node<E> l=last;
        final Node<E> newNode=new Node<E>(l,e,null);
        last=newNode;
        if(l==null){
            first=newNode;
        }else{
            l.next=newNode;
        }
        size++;
        modCount++;
    }
    ```

  - **删除元素的源码分析**

    ```java
    public E remove(){
        return removeFirst();//可见remove（）默认删除第一个元素
    }
    public E removeFirst(){
        final Node<E> f=first;
        if(f==null)
            throw new NoSuchElementException();
        return unLinkFirst(f);
    }
    private E unLinkFirst(Node<E> f){
        final E element=f.item;
        final Node<E> next=f.next;
        f.item=null;
        f.next=null;
        first=next;
        if(next==null){
            last=null;
        }else{
            next.prev=null;
        }
        size--;
        modCount++;
        return element;
    }
    ```

    

- # Set接口

  1. **Set接口的实现类的对象（下称Set接口对象）添加元素是无序的，即==添加元素和取出元素的方法不一致==，但取出元素的顺序是固定的，这在底层由算法控制**

  2. **Set接口对象中的元素是没有索引的**

  3. **Set接口对象中不允许添加重复的元素（何为重复，由程序员自己定义 **

     

     - ### HashSet

       1. **HashSet的底层实质上是HashMap，是数组+链表+红黑树**

          ```java
          //源码中的构造器
          public HashSet(){
              map=new HashMap<>();
          }
          
          ```

       2. **==注意==**:**关于Set接口对象中不允许添加重复的元素**

          ```java
          Set set=new HashSet();
          set.add=new String("zmx");//true
          set.add=new String("zmx");//false
          //思考：上下两个String并不是同一个对象，那么为什么不能添加呢？
          ```

          **解读：HashSet的底层源码分析：自己Debug去~~**

          **结论：**

          1. **HashSet底层是HashMap，是数组+链表+红黑树**

             ![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220228203412.png)

             ```java
             //简单模拟HashMap结构
             public class HashMapStructure{
                 Node[] table=new Node[16];//数组
             }
             
             class Node{
                 Object obj;
                 Node next;//链表
                 
                 public Node(Object obj,Node next){
                     this.obj=obj;
                     this.next=next;
                 }
             }
             ```

             

          2. **添加一个元素时，先得到该元素的hash值==（通过对该对象的hashCode值处理得到）==，将hash值用算法处理得到该元素的索引值**

          3. **通过索引值找到索引位置， 看是否已经存在元素**

          4. **如果不存在，则直接加入**

          5. **如果存在，再==调用equals==比较，如果相同，则放弃添加；如果不同，则放到该索引位置对应的链表的最后**

          6. **在JAVE8中，如果某条链表的节点个数到达8，并且table的大小到达64，就会进行树化**

          7. **如果链表个数不小于8但table表的大小小于64，则会对table进行两倍扩容**

          

          **综上所述，当将程序员自己编写的类加入HashSet之前，往往要==重写该类的hashCode（）和equals（）==**

          **练习题：**

          ```java
          //创建一个一个Employee类，有name和age属性
          //要求当name和age相同时，被认为是相同员工，不能重复加入
          
          public class Employee{
              
              String name;
              int age;
              
              public Employee(String name,int age){
                  this.name=name;
                  this.age=age;
              }
              //下面重写hashCode（）和equals（）
              //IDEA提供快捷键帮助编写：alt+insert 或右键->generate
              
              public int hashCode(){
                  return OBjects.hash(name,age);
              }
              
              public boolean equals(Object o){
                  if(o==this)return true;
                  if(o==null||getClass()!=o.getClass())return false;
                  Employee p=(Employee)o;
                  return p.age=age&&Objects.equals(name,p.name);
              }
              
          }
          ```

          

       3. ### Set集合的遍历方式

          - **因为Set接口继承自Collection,因此可以使用Iterator进行遍历**
          - **forEach增强for循环**
          

     

     - ### LinkedHashSet

       - **extends HashSet**
       - **底层是一个LinkedHashMap,维护了一个数组加双向链表**
       - **添加元素的原则与HashSet类似，但同时使用双向链表维护了元素的次序，使得元素看上去是以加入顺序保存的，因此，==LinkedHashSet中加入与取出的顺序是一致的==**
       - ![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220305171836.png)

     
     
     
     
     
     - ## TreeSet
     
       - **底层是TreeMap,跟HashSet与HashMap类似**
     
       - **TreeSet可以使用无参构造器，这样没有排序功能**
     
       - **当使用有参构造器时，传入一个Comparator来确定排序的规则**
     
       - **Comparator由程序员自己实现**
     
         ```java
         TreeSet treeSet=new TreeSet(new Comparator(){
             @Override
             public void Compare(Object o1,Object o2){
                 return (String)o1.compareTo((String)o2);
             }//返回0即是相等
         })
         ```
     
       - **传入相同的key（相同与否也是由Comparator决定）时，底层源码会用新value覆盖之前的value，key不做修改，但TreeSet的value均为null**
     
         ```java
         //截取部分源码
         else{//如果添加元素时发现key相同
             return t.setValue(value);
         }
         ```

         
     
     
     
     - ## Map接口
     
       **双列集合，用于保存具有映射关系的数据：Key-Value**
     
       ### **HashMap概述：**
     
       1. **基本结构在HashSet中已经介绍过，是数组+链表+红黑树**
       2. **Key-Value可以使任何类型的数据（未使用泛型），在底层会被封装到HashMap$Node中**
       3. **由debug追源码可知，Map中的key不允许重复，而value是允许重复的**
       4. **添加元素时，当key重复而value不同时，新添加的value会替换掉原来的value**
       5. **key可以为null，同样不能重复**
       6. **key对value存在单一映射关系，即总能通过指定的key找到对应的value**
       7. **添加元素的方法：put（）**
     
       
     
       - ## 源码分析（关于K-V在底层的存储）
     
         - **K-V最后在底层的存储是：HashMap$Node node=newNode(hash,key,null);**(newNode之间没有漏空格，newNode是方法（确信)
           - **Node实现了Entry接口**
         - **同时，==为了方便遍历==，HashMap还维护了一个EntrySet集合,集合名为entrySet，该集合存放Entry<K,V>类型的对象，即EntrySet<Entry<k,v>>;**
           - **其实，Entry是一个接口，而接口不能实例化，因此，实际上ertrySet中存放的是==上面所述的==实现了Entry接口的Node对象（接口的多态）**
           - **方法entrySet（）：返回Map的entrySet对象**
         -  **同样的，为了方便遍历，底层还维护了KeySet、Values分别维护key、value数据，实际上与EntrySet一致，都是不直接存储，但是有对Node的指向**

       - ## *Map接口的遍历方式*：
     
         - **keySet结合get（）**
     
           ```java
           Set keys=map.keySet();//keySet方法返回keySet对象
           for(Object key:keys){
               System.out.println(key+"::"map.get(key));
           }
           ```
     
         - **Iterator迭代器**
     
           - **keySet实现了Collection接口，因此可以使用迭代器**
     
         - **entrySet结合getKey()、getValue()**
     
           ```java
           Set entrySet=map.entrySet();
           for(Object entry:entrySet){
               Map.Entry m=(Map.Entry)entry;
               System,.out,println(m.getKey()+":"m.getValue());
           }
           ```
     
           - **同样，entrySet也实现了Collection接口，也可以使用Iterator迭代器**
     
     
     
     
     
     ### HashTable
     
     - **概述**：
       1. **HashTable存放的是K-V类型数据**
       2. **key不能重复，value可以重复**
       3. **当key相同而value不同时，新的value会覆盖之前的value**
       4. **key和value都不能为null，==这点与HashMap不同==**
       5. **HashTable是线程安全的，HashMap不是**
     
     
     
     
     
     ## TreeMap
     
     - **同样可以选择无参构造器或传入一个Comparator来进行排序**
     
     - **传入相同元素（相同与否也是由Comparator决定）时，底层源码会用新value覆盖之前的value值。key不做修改**
     
       ```java
       //截取部分源码
       else{//如果添加元素时发现key相同
           return t.setValue(value);
       }
       ```
     
       



## 集合选型规则

**由开发需求确定**

1. **先判断存储的类型**
2. **单列数据类型：Collection接口**
   1. **允许重复：List**
      - **改查较多：ArrayList**
      - **增删较多：LinkedList**
   2. **不允许重复：Set**
      - **无序：HashSet**
      - **存入与取出顺序一致：LinkedHashSet**
      - **排序：TreeSet**
3. **双列数据类型：Map接口**
   1. **key无序：HashMap**
   2. **key有序：TreeMap**
   3. **存入与取出顺序一致：LinkedHashMap**







# DAY17

## 泛型

**问题引出：==有了多态，为什么还要泛型？==**

1. **多态不能对对象的类型进行约束**
2. **多态需要使用向下转型，效率低下**

**自定义泛型类**

1. **基本语法**

   ```java
   public class Car<T,E,R...>{
       T t;
       E e;
       R r;
       ...
   }
   ```

2. **注意**

   1. **使用泛型的数组不能初始化，因为==泛型擦除==**
   2. **静态成员不可以使用泛型**
      - **因为==泛型类的泛型是在创建对象时确定的==，而静态成员与类的加载有关，当类被加载时，对象还未被创建**
   3. **当泛型类未使用泛型时（即没有传入T、E、R），泛型默认为Object**

**自定义泛型接口**

1. **基本语法**

   ```java
   interface I<T,R...>{
   }
   ```

2. **注意**

   1. **静态成员同样不能初始化**
   2. **没有指定类型时，同样默认为Object**
   3. **泛型接口类型在==继承接口==或==实现接口==时确定**

**自定义泛型方法**

1. **基本语法**

   ```java
   //修饰符 <T,R.> 返回类型 方法名(参数列表){
   }
   ```

2. **注意**

   1. **泛型方法既可以定义在泛型类中，也可以定义在普通类中**

   2. **泛型方法的类型在调用时被确定**

      ```java
      zmx.eat("chips",3);
      
      public <E,T> void eat(E e,T,t){
          ...
      }
      ```

   3. **public void eat(E e){},声明时没有<T,E..>,==不是泛型方法，而是使用了泛型==**

      - **使用了泛型的方法一定是定义在泛型类中，并使用了泛型类的泛型类型**

        ```java
        public class Animal<E e>{
            public void eat(E e){//true
               ... 
            }
            public void play(R r){//false
                ...
            }
        }
        ```

- **泛型的继承和通配符**

  - **泛型==不具有继承性==**

    ```java
    Object str=new String("abc");//true
    List<Object> list=new ArraysList<String>();//false
    ```

  - **<?>表示任意泛型类型**

  - **<? extends A>表示支持A和A的子类，规定了泛型的上限**

  - **<? super B>表示支持B和B的父类，规定了泛型的下限**



- ==**关于泛型擦除**==

  ## 泛型擦除概念[#](https://www.cnblogs.com/hongdada/p/13993251.html#泛型擦除概念)

  Java的泛型是伪泛型，这是因为Java在**编译期**间，所有的泛型信息都会被擦掉，正确理解泛型概念的首要前提是理解类型擦除。Java的泛型基本上都是在编译器这个层次上实现的，在生成的字节码中是不包含泛型中的类型信息的，使用泛型的时候加上类型参数，在编译器编译的时候会去掉，这个过程成为类型擦除。
  例如：`List<String>` 和 `List<Integer>` 在编译后都变成 `List`。

  ## 证明泛型擦除[#](https://www.cnblogs.com/hongdada/p/13993251.html#证明泛型擦除)

  ```vhdl
           ArrayList<String> arrayList1=new ArrayList<String>();
  		arrayList1.add("abc");
  		ArrayList<Integer> arrayList2=new ArrayList<Integer>();
  		arrayList2.add(123);
  		System.out.println(arrayList1.getClass()==arrayList2.getClass());
  ```

  我们定义了两个ArrayList数组，不过一个是ArrayList泛型类型，只能存储字符串。一个是ArrayList泛型类型，只能存储整形。最后，我们通过arrayList1对象和arrayList2对象的getClass方法获取它们的类的信息，最后发现结果为true。说明泛型类型String和Integer都被擦除掉了，只剩下了 原始类型。

  ```csharp
  ArrayList<Integer> arrayList3=new ArrayList<Integer>();
  		arrayList3.add(1);//这样调用add方法只能存储整形，因为泛型类型的实例为Integer
  		arrayList3.getClass().getMethod("add", Object.class).invoke(arrayList3, "asd");
  		for (int i=0;i<arrayList3.size();i++) {
  			System.out.println(arrayList3.get(i));
  		}
  ```

  在程序中定义了一个ArrayList泛型类型实例化为Integer的对象，如果直接调用add方法，那么只能存储整形的数据。不过当我们利用反射调用add方法的时候，却可以存储字符串。这说明了Integer泛型实例在编译之后被擦除了，只保留了 原始类型。

  ## 引用检查与编译[#](https://www.cnblogs.com/hongdada/p/13993251.html#引用检查与编译)

  说类型变量会在编译的时候擦除掉，那为什么我们往ArrayList arrayList=new ArrayList();所创建的数组列表arrayList中，不能使用add方法添加整形呢？

  **java编译器是通过先检查代码中泛型的类型，然后再进行类型擦除，在进行编译的。**

  ```csharp
           ArrayList<String> arrayList1=new ArrayList();
  		arrayList1.add("1");//编译通过
  		arrayList1.add(1);//编译错误
  		String str1=arrayList1.get(0);//返回类型就是String
  		
  		ArrayList arrayList2=new ArrayList<String>();
  		arrayList2.add("1");//编译通过
  		arrayList2.add(1);//编译通过
  		Object object=arrayList2.get(0);//返回类型就是Object
  		
  		new ArrayList<String>().add("11");//编译通过
  		new ArrayList<String>().add(22);//编译错误
  		String string=new ArrayList<String>().get(0);//返回类型就是String
  ```

  **类型检查就是针对引用的，会对这个引用调用的方法进行类型检测，而无关它真正引用的对象。**

  ## 泛型擦除与多态的冲突和解决方法[#](https://www.cnblogs.com/hongdada/p/13993251.html#泛型擦除与多态的冲突和解决方法)

  ### 泛型重载变重写？[#](https://www.cnblogs.com/hongdada/p/13993251.html#泛型重载变重写)

  ```typescript
  public class Pair<T> {
  	private T value;
  	public T getValue() {
  		return value;
  	}
  	public void setValue(T value) {
  		this.value = value;
  	}
  }
  
  public class DateInter extends Pair<Date> {
  	@Override
  	public void setValue(Date value) {
  		super.setValue(value);
  	}
  	@Override
  	public Date getValue() {
  		return super.getValue();
  	}
  }
  ```

  在这个子类中，我们设定父类的泛型类型为Pair，在子类中，我们覆盖了父类的两个方法，我们的原意是这样的：

  将父类的泛型类型限定为Date，那么父类里面的两个方法的参数都为Date类型

  ```typescript
      public Date getValue() {
  		return value;
  	}
  	public void setValue(Date value) {
  		this.value = value;
  	}
  ```

  实际上，**类型擦除后，父类的的泛型类型全部变为了原始类型Object，而子类的类型是Date，参数类型不一样，这如果实在普通的继承关系中，根本就不会是重写，而是重载**。

  本意是将父类中的泛型转换为Date类型，子类重写参数类型为Date的两个方法

  **实际上，虚拟机并不能将泛型类型变为Date，只能将类型擦除掉，变为原始类型Object。这样，我们的本意是进行重写，实现多态。可是类型擦除后，只能变为了重载。这样，类型擦除就和多态有了冲突。JVM知道你的本意吗？知道！！！可是它能直接实现吗，不能！！！**

- ## 擦除带来的问题

  Java 是通过擦除来实现把泛型类型实例关联到同一份字节码上的。

  编译器只为泛型类型生成一份字节码，从而节约了空间，但是这种实现方法也带来了许多隐含的问题。下面介绍几种常见的问题。

  #### 1) 泛型类型变量不能是基本数据类型

  泛型类型变量只能是引用类型，不能是 Java 中的 8 种基本类型（`char`、`byte`、`short`、`int`、`long`、`boolean`、`float`、`double`）。

  以 List 为例，只能使用 List<Integer>，但不能使用 List<int>，因为在进行类型擦除后，List 的原始类型会变为 Object，而 Object 类型不能存储 int 类型的值，只能存储引用类型 Integer 的值。

  #### 2) 类型的丢失

  通过下面一个例子来说明类型丢失的问题。

  ```
  class Test{    public void List<Integer> list){}    public void List<String> list){}}
  ```

  上述代码中，编译器认为这个类中有两个相同的方法（方法参数也相同）被定义，因此会报错，主要原因是在声明 List<String> 和 List<Integer> 时，它们对应的运行时类型实际上是相同的，都是 List，具体的类型参数信息 String 和 Integer 在编译时被擦除了。

  正因为如此，对于泛型对象使用 instanceof 进行类型判断的时候就不能使用具体的类型，而只能使用通配符`？`，示例如下所示：

  ```
  List<String> list=new ArrayList<String>();if( list instanceof ArrayList<String>) {}  //编译错误if( list instanceof ArrayList<?>) {}  //正确的使用方法
  ```

  #### 3) catch中不能使用泛型异常类

  假设有一个泛型异常类的定义 MyException<T>，那么下面的代码是错误的：

  ```
  try{}catch (MyException<String> e1)
  ```

  catch ( MyException<Integer>e2){…}

  因为擦除的存在，MyException<String> 和 MyException<Integer> 都会被擦除为 MyException<Object>，因此，两个 catch 的条件就相同了，所以这种写法是不允许的。

  此外，也不允许在 catch 子句中使用泛型变量，示例代码如下所示：

  ```
  public <T extends Throwable> void test(T t) {    try{        ...    }catch(T e) {  //编译错误        ...    } catch(IOException e){    }}
  ```

  假设上述代码能通过编译，由于擦除的存在，T 会被擦除为 Throwable。由于异常捕获的原则为：先捕获子类类型的异常，再捕获父类类型的异常。

  上述代码在擦除后会先捕获 Throwable，再捕获 IOException，显然这违背了异常捕获的原则，因此这种写法是不允许的。

  #### 4) 泛型类的静态方法与属性不能使用泛型

  由于泛型类中的泛型参数的实例化是在实例化对象的时候指定的，而静态变量和静态方法的使用是不需要实例化对象的，显然这二者是矛盾的。

  如果没有实例化对象，而直接使用泛型类型的静态变量，那么此时是无法确定其类型的。



# DAY17

## **JUnit简单使用**

- **介绍：JUnit是一个java语言的单元测试框架**

- **当我们需要测试类的方法时，往往需要将其写在main方法中调用，但使用JUnit则不必**

- **使用**

  ![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220309214146.png)

  **在Test处alt+enter，选择合适的JUnit版本添加，比如JUnit5.7.0**







# DAY18

## Java坐标体系



### 基本介绍

- **java坐标体系以左上角为原点，以像素作为单位**
- **像素：像素是密度单位，不是长度单位。同一块屏幕，可以分为1080/720，也可以分为720/480**



### **基本结构**

```java
public class MyFrame extends JFrame{
    private myPanel;
    
    public MyFrame(){
        myPanel=new MyPanel();
        this.add(myPanel);
        this.setSize(500,500);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisuable(true);
    }
}

class MyPanel extends JPanel{
    
   @Override
    public void paint(Graphi g){
        super.paint(g);//必须要有这一行，调用父类的paint()
        g.drawLine(x,y,xx,yy);
    }
}
```

**关于paint()方法**

- **paint（）绘制组件的外观**
- **paint（）会在以下情况中被自动调用**
  1. **组件第一次在屏幕中显示的时候**
  2. **先最小化，再最大化时**
  3. **窗口大小变化时**
  4. **调用repaint()时**







# DAY19

## 线程

- **线程的两种使用方式：本质一致，均是将要执行的任务写在==run==方法中**

  1. **继承Thread类**

     ```JAVA
     public static void main(String[] args){
         Mythread t=new MyThread();
         t.start();//启动新线程并在底层调用run方法
     }
     class MyThread extends Thread{
         @Override
         public void run(){
             ...
         }
     }
     ```

  2. **实现Runable接口**

     ```java
     public static void main(String[] args){
         Thread t=new Thread(new MyRunable());
         t.start();//启动新线程并在底层调用run方法
     }
     class MyRunable implements Runable{
         public void run(){
             ...
         }
     }
     ```

     

- **细节**

  1. **为什么不直接调用run方法**？
     - **源码中start方法中调用了start0方法，这个方法是实现多线程的关键，而run只是一个普通方法**
     - **因此，直接调用run方法，只会在main线程中执行run中的任务，并没有达到多线程的目的**
  2. **main线程的结束不会影响新开的线程，二者是并发关系**

- **Thread常用方法**

  1. **yield（）：礼让**
     - **让出cpu，让其他线程先执行，但礼让的时间不确定，也不一定礼让成功，如果cpu资源充足，则就不会礼让成功**
  2. **join（）：插队**
     - **插队的线程一旦插队成功，则一定会执行完所有的任务**
     - **如：在main线程中执行:t1.join()，则一定会t1执行完成后再执行main线程**
  3. **interrupt():中断**
     - **一般用于中断线程的休眠**
     - **休眠中的线程被中断是会抛出一个InterruptedException，可以catch并添加自己的业务代码**

- **工作线程与守护线程**

  - **工作线程：当线程的工作完成或以被通知的方式完成**

  - **守护线程：一般是为工作线程服务的，当工作线程结束时，守护线程自动结束**

  - **典型守护线程：垃圾回收机制**

  - **示例**

    ```java
    //在main线程中
    MyThread t=new MyThread();
    t.setDaemon(true);//这句写在start之前
    t,start();
    //此时t是main的守护线程，当main结束，t则自动结束
    ```
    
    

- **线程的生命周期**

  - **new**

  - **Runable**

  - **Time-waiting**

  - **Waiting**

  - **Blocked**

  - **Ternimated**

  - ==**状态转换图**==

    ![](C:\Users\38156\Desktop\工具资料\工具图片\【零基础 快速学Java】韩顺平 零基础30天学会Java_哔哩哔哩_bilibili - Google Chrome 2022_3_15 16_41_49.png)









# DAY20

## IO流

**导图**

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220316225656.png)

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220319151106.png)

- **概念**

  - **流：数据在数据源（文件）和程序（内存）之间经历的路径**
    - **输入流：数据从数据源到程序**
    - **输出流：数据从程序到数据源**

- **创建文件的相关构造器及方法**

  - **new File(String pathName)//根据路径名创建**

  - **new File(File parent,String child)//根据父目录以及子路径创建**

  - **new File(String parent,String child)//根据父目录加子路径创建**

  - **creatNewFile();**

  - ```java
    //方式一：
    String path=new File("d:\\zmx.txt");
    File file=new File(path);
    
    //方式二：
    String parentPath="e:\\";
    File parent=new File(parent);
    String childPath="zmx.txt";
    File file=new File(parent,child);
    
    //方式三
    String parentPath="e:\\";
    String childPath="zmx.txt";
    File file=new File(parentPath,childPath);
    
    //最后
    file.createNewFile()
    ```

    **==注意==：创建完file之后，file还仅仅停留在内存中，要通过==creatNewFile()==才能联系到硬盘**
  
- **文件操作**

  1. **首先记住，目录也被当做文件**
  2. **创建目录：（由file调用）**
     1. **mkdirs（）：创建多级目录**
     2. **mkdir（）：创建一级目录**



## IO流原理和分类

- **字节流：FileInputStream、FileOutputStream，按字节操作**
- **字符流：FileReader，FileWriter，按字符操作**





**InputStream结构图**

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220317151100.png)





- **FileInputSteam**

  **直接上代码学习**

  ```java
  public void readLine(){
          String filePath="d:\\zmx.txt";
          FileInputStream fileInputStream=null;
          int readConTent;
          try {
              fileInputStream=new FileInputStream(filePath);
              while((readConTent=fileInputStream.read())!=-1){
                  System.out.println((char)readConTent);
              }
          } catch (IOException e) {
              e.printStackTrace();
          }finally {
              try {
                  fileInputStream.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
  ```

  - **构造方法：FileInputStream fileInputStream=new FileInputStream(filePath);**
  - **read():每次读取下一个==字节==的数据，若不存在则返回-1**
    - **因为读取的是字节信息，而一个汉字在UTF-8中有三个字节，因此==这种方式不能读取汉字==。否则乱码**
  - **close():流是路径，如果用完不及时不关闭流，则会导致可能有多个流指向同一个文件，造成资源的浪费**

  

  **==read(Byte[])==可以一次读取多个字节**

  - **read(byte[]):最多可以一次读取byte[].length个字节，最后返回实际读取的字节数，如果读取完毕则返回-1**
  - **new String(byte[],int beginIndex,int endIndex):转成字符串**

  **使用read(Byte[])提升效率**

  ```java
  public void readLine(){
          String filePath="d:\\zmx.txt";
          FileInputStream fileInputStream=null;
          byte[] by=new byte[8];
          int readLength=0;
          try {
              fileInputStream=new FileInputStream(filePath);
              while((readLength=fileInputStream.read(by))!=-1){
                  System.out.println((new String(by,0,readLength));
              }
          } catch (IOException e) {
              e.printStackTrace();
          }finally {
              try {
                  fileInputStream.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
                                     
  ```
  



**OutputSteam结构图**

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220318130314.png)

- **FileOutputStream**

  **上代码**

  ```java
  public void writeFile(){
          String filePath="d:\\zjm.txt";
          FileOutputStream fileOutputStream=null;
          try {
              fileOutputStream=new FileOutputStream(filePath,true);
  //            fileOutputStream.write('z');//输入单个字符
              String str="hello,world";
              fileOutputStream.write(str.getBytes());//输入字符串
          } catch (IOException e) {
              e.printStackTrace();
          }finally {
              try {
                  fileOutputStream.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
  ```

  - **构造方法：  FileOutputStream(String filePath,boolean append)//==append为true，则在文件末端写入，为false，则在文件开头写入==**
  - **指定路径不存在时会自动创建**
  - **write(int a):输入单个字符**
  - **write(byte[] bytes):输入字符串**
  - **write(byte[] bytes,int off,int len):在bytes的off偏移位置开始取出len长度的字节并输出**
  - **getBytes():字符串转字节数组**



- **FileReader**

  - **构造方法：new FileReader(File/String)**

  - **read（）：每次读取一个字符并返回，若文件尾则返回-1**

  - **read(char[]):批量读取多个字符到字符数组中，并返回读取字符的个数，若文件尾则返回-1**

  - **相关api：new String(char[] chs,int off,int length)**

  - ```java
     public void readFile(){
            String filePath="d:\\zmx.txt";
            FileReader fileReader=null;
            try {
                fileReader=new FileReader(filePath);
                //单个字符读取
    //            int ch=0;
    //            while((ch=fileReader.read())!=-1){
    //                System.out.print((char)ch);
    //            }
    
                //多个字符读取
                int readLength=0;
                char[] chs=new char[8];
                while((readLength=fileReader.read(chs))!=-1){
                    System.out.print(new String(chs,0,readLength));
                }
            } catch (IOException e) {
                e.printStackTrace();
            }finally {
                try {
                    fileReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    ```






- **FileWriter**

  - **构造方法：new FileWriter(File/String,boolean append)**

  - **write(int):写入单个字符**

  - **write(char[]):写入字符数组**

  - **write(char[] ,int off,int len):写入字符是数组的指定部分**

  - **write(String):写入字符串**

  - **write(String str,int off,int len):写入字符串的指定部分**

  - **一定要flush()或close()，==只有flush（）或close（）之后，字符才会被真正写入到流==**

  - ```
    //懒得写了，都差不多
    ```

    

**文件拷贝**

**文件拷贝一定是边读遍写的，不是先完全读完再去写入**

```java
public void copyFile(){
        String srcPath="d:\\zjm.txt";
        String destPath="d:\\zmx.txt";
        FileInputStream fileInputStream=null;
        FileOutputStream fileOutputStream=null;

        try {
            fileInputStream=new FileInputStream(srcPath);
            fileOutputStream=new FileOutputStream(destPath,true);
            byte[] bytes=new byte[10];
            int readLength=0;
            while((readLength=fileInputStream.read(bytes))!=-1){
                fileOutputStream.write(bytes,0,readLength);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                fileOutputStream.close();
                fileInputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```







## 节点流和处理流

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220320142611.png)

- **节点流可以从一个特定的数据源读写数据，如：FileReader,FileWriter**
- **处理流（也叫包装流）：是"连接"在已经存在的流（节点流或处理流）之上，为程序提供更强大的读写功能，如BufferedReader,BufferedWriter**
  - **以BufferedReader为例，其有一个Reader类型的成员变量，因此BufferedReader可以接收任意一个Reader的子类对象**



- **BufferedReader**

  - **==构造方法==：new BufferedReader(Reader in)**
    - ==意味着可以传入任意一个Reader类的子类==
  - **readLine():读取一行并返回读到的String，否则返回null**
  - **readLine()方法在进行读取一行时，只有遇到回车(\r)或者换行符(\n)才会返回读取结果，并且==对返回的读取结果进行擦除(\r)或(\n)的操作==，因此在循环输出时使用println**
  
  ```java
   public void readFile(){
          String filePath="d:\\zmx.txt";
          BufferedReader bufferedReader=null;
          try {
              String str=null;
              bufferedReader=new BufferedReader(new FileReader(filePath));
              while((str=bufferedReader.readLine())!=null){
                  System.out.println(str);//因为擦除了换行符，所以这里使用println而不是print
              }
          } catch (IOException e) {
              e.printStackTrace();
          }finally {
              try {
                  bufferedReader.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
           
          }
      }
  ```
  
  
  
- **BufferedWriter**

  ```
  //懒得写（，都差不多
  ```

  - **记得writer.newLine();**
  - **记得writer.flush()**



## 对象处理流

**需求：将int num=100保存到文件中，注意并不止保存100这个数值，还要保存int数据类型；将Dog dog=new Dog("大黄"，10)保存到文件中；同时还要保证能从文件中恢复**

**上面的要求即是：能够将基本数据类型进行序列化或反序列化**

- **序列化和反序列化**
  1. **在保存数据时，保存数据的数值和数据类型**
  
  2. **在恢复数据时，恢复数据的数值和数据类型**
  
  3. **要让某个对象或数据类型可以被序列化或反序列化，==就要这个对象及其成员属性实现如下两接口之一==**
     
     1. **==Serializable==:标记接口，没有方法需要实现**
     2. **Externalizable:该方法有方法需要是实现，因此一般使用Serializable**
     
  4. **序列化的对象建议添加SerialVersionUID属性，为了提高版本的兼容性**
  
  5. **static或transient修饰的属性不可以被序列化**
  
  6. **序列化具备可继承性，即，若一个类实现了序列化在，则其子类默认实现了可序列化**
  
     





- **ObjectOutputStream**

  **上代码**

  ```java
  public void writer(){
          String filePath="d:\\data.dat";
          ObjectOutputStream objectOutputStream=null;
  
          try {
              objectOutputStream=new ObjectOutputStream(new FileOutputStream(filePath));
  
              objectOutputStream.writeInt(100);
              objectOutputStream.writeBoolean(true);
              objectOutputStream.writeChar('a');
              objectOutputStream.writeUTF("詹美祥zmx");
              objectOutputStream.writeObject(new Dog("大黄",10));
  
          } catch (IOException e) {
              e.printStackTrace();
          }finally {
              try {
                  objectOutputStream.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
  }
  
  class Dog implements Serializable{
      String name;
      int age;
  
      public Dog(String name, int age) {
          this.name = name;
          this.age = age;
      }
  }
  ```
  
  

- **ObjectInputStream**

  **反序列化的顺序要和序列化的顺序保持一致**

  **方法：例：将writeInt换成readInt**

  **注意：readObject()返回的是Object类型**

  



## 标准输入输出流

- **System.in  标准输入  类型：InputStream  默认设备：键盘**
- **System.out  标准输出  类型：PrintStream  默认设备:显示器**





## 转换流

- **InputStreamReader:可以将InputStream包装成字符流，并选择指定的编码格式（可以不选，下同）**
  - **构造方法：new InputStreamReader(InputStream,CharSet**)

- **OutputStreamWriter:可以将OutputStream包装成字符流，并根据指定的编码格式**
  -   **构造方法：new OutputStreamWriter(OutputStream,CharSet)**





## 打印流

- **PrintStream**
- **PrintWriter**



## Properties

- **Properties不是一种流，他是一种集合，继承自HashTable**

- **在开发中，我们常常用properties类型的文件储存信息，这也是一种==K-V==的存储模式，如下**

  ![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220320173144.png)

- **Properties对象的作用就是：方便的读取properties文件，并对其进行的修改，或者是创建Properties文件**

  -   **信息的默认类型是String**

  - **中文在properties对象中会以unicode码的形式保存**

- **常用方法：**

  - **load(Read in/InputStream in):加载properties文件的K-V值到properties中**
  - **list(PrintWriter out/PrintStream out):将数据显示到指定设备**
  - **getProperty(key):根据键获取值**
  - **setProperty(key,value):设置键值对到Properties对象**
  - **store(Writer writer/OutputStream out,String comment):将Properties对象中的键对保存到对应的properties文件中，comment是对文件的描述，将写在文件的第一行**

- **实例**

  ```java
  public class Properties_ {
      public static void main(String[] args) throws IOException {
          Properties properties=new Properties();
          properties.load(new FileReader("src\\PAPER\\mysql.properties"));
          properties.list(System.out);
          properties.store(new FileWriter("src\\PAPER\\mysql.properties"),"mysql用户信息");
      }
  }
  ```







# DAY20



## ip地址

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220323092824.png)

- **A类：0.0.0.0-127.255.255.255**
- **B类：128.0.0.0-191.255.255.255**
- **C类：192.0.0.0-223.255.255.255**
- **D类：224.0.0.0-239.255.255.255**
- **E类：240.0.0.0-247.255.255.255**



## 端口



## 网络通信协议

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220323094018.png)

![](C:\Users\38156\Desktop\工具资料\工具图片\QQ图片20220323094107.png)

## **TCP字节流编程**



- **InetAddress类**

  - **常用方法：**
    1. **获取本机InetAddress对象：getLocalHost();**
    2. **根据指定主机名，获取InetAddress对象：getByName(String)**
    3. **根据域名返回InetAddress对象：getByName(String)**
    4. **通过InetAddress对象，获取对应的地址：getHostAddress()**
    5. **通过InetAddress对象，获取对应的主机名或者域名：getHostName()**

- **TCP编程思路**
  - **客户端和服务器端各有一个socket对象**
  
  - **客户端**
    1. **连接服务端(服务端ip,服务端端口)**
    2. **连接上后，生成Socket，通过socket.getOutputStream()获取输出流**
    3. **通过获取的输出流输出数据**
    4. **关闭流、socket**
  - **服务端**
    1. **在本机的一个空闲端口监听，等待连接**
    2. **没有客户端连接端口时，服务器端会处于阻塞状态**
    3. **有客户端连接时，返回一个socket对象，通过socket.getInputStream()获取输入流**
    4. **关闭流、socket、serverSocket**
  - **==注意==**
    - **每次输出完内容之后，必须调用==socket.shutdownOutput()==来作为结束标记，否则内容将无法传输成功**
    - **也可以在write之后，添加一行==writer.newLine()==（这个方法实际就是添加一个换行符）作为结束标记，不过这要求接收方==必须使用readLine==来接收**
    - **在TCP传输的过程中，在客户端，其实也是靠端口来进行连接的，只是该端口号是由TCP协议随机生成**

- **StreamUtils_（自己的一个手写工具类）**

  - **功能：：用于将输入流转化为字节数组，即可以把文件的内容读取到一个字节数组**

  - ```java
    public class StreamUtils_ {
        /*
        功能：用于将输入流转化为字节数组
        即可以把文件的内容读取到一个字节数组
         */
    
        public static byte[] streamToByteArray(InputStream in) throws IOException {
            ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
            byte[] buf=new byte[1024];
            int readLength=0;
            while((readLength=in.read(buf))!=-1){
                byteArrayOutputStream.write(buf,0,readLength);
            }
            byte[] res=byteArrayOutputStream.toByteArray();
            byteArrayOutputStream.close();
            return res;
        }
    }
    
    ```





- **netstat指令**
  - **netstat -an:可以查看当前主机网络情况，包括端口监听情况和网络连接情况**
  - **netstat -an | more:可以分页显示**
  - **netstat -anb:在上述基础上显示端口连接的具体程序（需要管理员权限）**





## UDP编程

- **类DatagramSocket和类DatagramPacket实现了基于UDP协议网络程序**

- **UDP数据报由DatagramSocket接收和发送，系统不保证数据报能不能送到以及什么时候送到**

- **数据报封装在DatagramPacket对象中，在数据报中包含了发送端的ip地址和端口号以及接收端的ip地址和端口号**

- **UDP协议的特点：由于每一份数据报中都包含完整信息，因此不需要建立发送端和接收端的连接**

- **每一份数据报最大不超过64kb**

- **基本流程**

  1. **核心类：DatagramSocket,DatagramPacket**

  2. **建立发送端，接收端**

  3. **建立数据报**

  4. **调用Socket的发送，接收方法**

  5. **关闭socket**

  6. **==测试==**

     ```java
     //发送测试端
     public class ServerA {
         public static void main(String[] args) throws IOException {
     
             //选择6666端口作为packet的接收端口，此时作为发送端，所以没有用到
             DatagramSocket datagramSocket = new DatagramSocket(6666);
             byte[] buf=new byte[1024];
             buf="原来你也玩原神".getBytes();//转成字节数组
     
             //当packet用于发送时，要指定接收端的IP地址以及端口号
             DatagramPacket datagramPacket = new DatagramPacket(buf, buf.length, InetAddress.getLocalHost(), 6665);
     
             //发送
             datagramSocket.send(datagramPacket);
     
             //关闭socket
             datagramSocket.close();
         }
     }
     ```

     ```java
     //接收测试端
     public class ServerB {
         public static void main(String[] args) throws IOException {
     
             //选择6665端口作为packet的接收端口
             DatagramSocket datagramSocket = new DatagramSocket(6665);
             byte[] buf=new byte[1024*64];
     
             //创建要用于接收数据报的packet
             DatagramPacket datagramPacket = new DatagramPacket(buf, 0, buf.length);
     
             //等待接收数据报，如果没有数据报传来，将会一直阻塞在这里
             datagramSocket.receive(datagramPacket);
     
             //获取接收到的数据包的实际长度
             int length=datagramPacket.getLength();
     
             //getData()：获取真正需要的数据
             String str=new String(datagramPacket.getData(),0,length);
             System.out.println(str);
     
     
             //关闭socket
             datagramSocket.close();
         }
     }
     
     ```

     

