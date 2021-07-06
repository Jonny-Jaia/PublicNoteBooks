# C++核心编程

```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
    
    return 0;
}
```



 ## 1.内存分区模型

**4个区域：代码区、全局区、栈区、堆区**

![](https://i.imgur.com/7d5FZV9.png)

### 1.1程序运行前

编译后->exe可执行程序

未执行程序前：

**代码区：**共享、只读

**全局区：**全局、静态变量

![](https://i.imgur.com/2SvC1G5.png)

示例

```c++
//全局区变量
int g_a = 10;
int g_b = 10;
//const修饰的全局变量
const int c_g_a = 10;
const int c_g_b = 10;
int main() {
    //普通局部变量
    int a = 10;
    int b = 10;
    //静态变量
    static int s_a = 10;
    static int s_b = 10;

    cout<<"局部地址a:"<<(int)&a<<endl;
    cout<<"局部地址b:"<<(int)&b<<endl;
    cout<<"全局地址g_a:"<<(int)&g_a<<endl;
    cout<<"全局地址g_b:"<<(int)&g_b<<endl;
    cout<<"静态地址s_a:"<<(int)&s_a<<endl;
    cout<<"静态地址s_b:"<<(int)&s_b<<endl;
    //常量
    //字符串常量
    cout<<"字符串常量地址："<<(int)&("hello world")<<endl;
    //const修饰的常量
    //const修饰的全局变量
    cout<<"const修饰的全局变量c_g_a地址"<<(int)&c_g_a<<endl;
    cout<<"const修饰的全局变量c_g_a地址"<<(int)&c_g_b<<endl;
    //const修饰的局部变量
    const int c_l_a = 10;
    const int c_l_b = 10;
    cout<<"const修饰的局部变量c_l_a地址"<<(int)&c_l_a<<endl;
    cout<<"const修饰的局部变量c_l_b地址"<<(int)&c_l_b<<endl;
    return 0;
}
```

![](https://i.imgur.com/4OnPVRp.png)

### 1.2程序运行后

**栈区：**编译器释放、存放函数参数局部变量等；

![](https://i.imgur.com/8Vl3H6U.png)



```c++
#include <iostream>
using namespace std;
//不要返回局部变量的地址
int * func(){
    int a = 10;//局部变量，存放在栈区，执行后自动释放
    return &a;//地址
}

int main(){
    int *p = func();//在vs中可以打印第一次，vs编译器会对数据做一次保留，但clion不会
    cout<<*p<<endl;
    cout<<*p<<endl;
    return 0;
};
```

**堆区：**程序员分配释放；new在堆区开辟内存

![](https://i.imgur.com/9SuFoSU.png)

```c++
//
// Created by JSQ on 2021/6/27.
//
#include <iostream>
using namespace std;
int * headfunc(){
    //利用new关键字
    int *p = new int(10);
    return p;
}
int main(){
    //在堆区开辟数据
    int *p = headfunc();
    cout<<*p<<endl;
    return 0;
}
```

### 1.3 new操作符

![](https://i.imgur.com/uXFTRgc.png)

```c++
//
// Created by JSQ on 2021/6/27.
//
#include <iostream>
using namespace std;
//new的基本语法
int * newfunc(){
    //利用new关键字，new返回的是该数据类型的指针
    int *p = new int(10);
    return p;
}
void newtest(){
    int *p = newfunc();
    cout<<*p<<endl;
    cout<<*p<<endl;
    cout<<*p<<endl;
    //堆区的数据由管理员开辟和释放
    //释放内存
    delete p;
    cout<<*p<<endl;
    // 内存以释放再访问就是非法操作
}
void newtest02(){
    //创建10整型数据的数组，在堆区
   int *arr =  new int[10];//10个元素
   for(int i=0;i<10;i++){
       arr[i] = i+100;
   }
    for(int i=0;i<10;i++){
        cout<<arr[i]<<endl;
    }
    //释放堆区数组
    //释放数组要加[]
    delete[] arr;
}
//在堆区利用new开辟数组
int main(){
    //在堆区开辟数据
    newtest02();
    return 0;
}

```



## 2. 引用

### 2.1引用的基本使用

**作用：**起别名

**语法：**`数据类型 &别名 = 原名`

```c++
#include <iostream>

using namespace std;

int main(){
    int a =10;
    //创建引用
    int &b = a;
    cout<<"a="<<a<<endl;
    cout<<"b="<<b<<endl;

    b =100;
    cout<<"a="<<a<<endl;
    cout<<"b="<<b<<endl;
}
```

### 2.2引用的注意事项

- 引用必须要初始化
- 引用一旦初始化后就不可更改

![](https://i.imgur.com/mrEExub.png)

```c++
#include <iostream>

using namespace std;
int main(){
    int a = 10;
    //引用必须初始化
//    int &b;错误必须初始化
    int &b = a;
    //引用在初始化后不可改变
    int c = 30;
    b = c;//赋值。不是更改引用
    cout<<"a="<<a<<endl;
    cout<<"b="<<b<<endl;
    cout<<"c="<<c<<endl;

}
```

### 2.3引用做函数参数

**作用**：函数传参时，可以利用引用技术让形参修饰实参

**优点：**可以简化指针修改实参

```c++
#include <iostream>

using namespace std;
//交换函数
//1 值传递
void mySwap01(int a, int b){
    int temp = a;
    a = b;
    b = temp;
    cout<<"swap01 a="<<a<<endl;
    cout<<"swap01 b="<<b<<endl;
}
//2 地址传递
void mySwap02(int *a, int *b){
    int temp = *a;
    *a = *b;
    *b = temp;
    cout<<"swap02 a="<<*a<<endl;
    cout<<"swap02 b="<<*b<<endl;
}
//3 引用传递
void mySwap03(int &a, int &b){
    int temp = a;
    a = b;
    b = temp;
    cout<<"swap03 a="<<a<<endl;
    cout<<"swap03 b="<<b<<endl;
}
int main(){
    int a=10, b=20;
    cout<<"init a="<<a<<endl;
    cout<<"init b="<<b<<endl;
    mySwap01(a,b);//值传递，形参不会修饰实参
//    mySwap02(&a, &b);//地址传递，形参会修饰实参
    mySwap03(a,b);//引用传递，形参不会修饰实参
    cout<<"a="<<a<<endl;
    cout<<"b="<<b<<endl;
    return 0;
}
```
> 总结：通过引用参数产生的效果同按地址传递是一样的。引用的语法更清楚简单

### 2.4引用做函数返回值

**作用：**引用可以作为函数的返回值存在

**ps:返回局部变量引用**

**用法：**函数调用作为左值

```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>

using namespace std;
//引用做函数的返回值
//1 不要返回局部变量的引用
int& test012_4(){
    int a = 10;//局部变量存放在四区中的栈区
    return a;
}
//2 函数的调用可以作为左值
int& test022_4(){
    static int a = 10;//静态变量，存放在全局区，全局区数据在程序结束后系统释放
    return a;
}
int main(){
//    int &ref1 = test012_4();
//    cout<<"ref1="<<ref1<<endl;//报错，当前变量内存已经被释放，非法操作
    int &ref2 = test022_4();
    cout<<"ref2="<<ref2<<endl;//全局区数据，需要全部代码执行完才会被释放
    //函数的调用可以作为左值
    test022_4() = 100;
    cout<<"ref2="<<ref2<<endl;//全局区数据，需要全部代码执行完才会被释放
    return 0;
}
```
### 2.5引用的本质

**本质：**在c++内部实现是一个**指针常量**

![](https://i.imgur.com/TjBzTfy.png)

```c++
#include <iostream>

using namespace std;
void func2_5(int &ref){
    ref = 100;//ref 是引用，转换为*ref = 100
}
int main(){
    int a  = 10;
    int &ref = a;
    //自动转换危 int* const ref  = &a;
    //指针常量是指针指向不可改，也说明为什么引用不可更改
    ref = 20;
    //内部发现是引用，自动转换为*ref = 20;
    cout<<"a="<<a<<endl;
    cout<<"ref="<<ref<<endl;
    func2_5(a);
    return 0;
}
```
> 总结：C++推荐用引用技术，因为语法方便，引用本质是指针常量，但是所有的指针操作编译器都帮我们做了

### 2.6常量引用

**作用：**常量引用主要用来修饰形参，防止误操作

在函数形参列表中， 可以加`const修饰形参`,防止形参改变实参

```c++

```
## 3. 函数提高

### 3.1 函数默认参数

c++中，函数的形参列表中的形参是可以有默认值的

语法：`返回值类型 函数名（参数=默认值）{}`
```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>

using namespace std;
int func3_11(int a,int b=2,int c=3)
{
    return a+b+c;
}
//1. 某位置有了默认参数，该位置往后都必须有默认值
//2. 函数声明有默认参数，函数实现就不能有默认参数
//即声明和实现只能有一个默认参数，若都包含就存在二义性了
int func3_12(int a= 10,int b=100);
//int func3_12(int a= 10,int b=100){
//    return a+b;
//}
int main(){
//    func3_12(10,20);
    return 0;
}
```
### 3.2 函数占位参数

![](https://i.imgur.com/JdgW3N3.png)

```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>
using namespace std;
//占位参数
//目前阶段用不上
//占位参数也可以有默认参数
void func3_21(int a, int=10){
    cout<<"hello"<<endl;
}
int main(){
    func3_21(10);
    return 0;
}
```
### 3.3函数重载

![](https://i.imgur.com/KPc5oRi.png)



#### 3.3.1 函数重载概述
```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>
using namespace std;
//函数重载，函数名相同，提高复用性
//函数重载的满足条件
//1.同一个作用域
// 2.函数名称相同
// 3.函数参数类型不同/个数不同/顺序不同
//注意事项：函数的返回值不可作为函数的重载条件
void func3_3_1(){
    cout<<"func 的调用"<<endl;
}
void func3_3_1(int a){
    cout<<"func(a) 的调用"<<endl;
}
void func3_3_1(double b){
    cout<<"func(b) 的调用"<<endl;
}
void func3_3_1(int a,double b){
    cout<<"func(a,b) 的调用"<<endl;
}

int main(){
    func3_3_1();
    func3_3_1(10);
    func3_3_1(10.1);
    func3_3_1(10,10.1);
    return 0;
}
```
#### 3.3.2 函数重载注意事项

- 引用作为重载条件
- 函数重载碰到函数默认参数

```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>
using namespace std;
//ps：
// 1.引用作为重载条件
void func3_3_2(int &a){
    cout<<"func3_3_2(int &a)调用"<<endl;
}
void func3_3_2(const int &a){
    cout<<"func3_3_2(const int &a)调用"<<endl;
}
// 2.遇到默认参数-如下传一个参数调用均可通过，出现二义性，报错
//避免设置默认参数
void func3_3_2_2(int a){
    cout<<"func3_3_2(int a)调用"<<endl;
}
void func3_3_2_2(int a, int b=10){
    cout<<"func3_3_2(int a)调用"<<endl;
}
int main(){
    int a = 10;
    func3_3_2(a);
    func3_3_2(10);//当前常量10是非法空间，因此会走const
    func3_3_2_2(10);
    
    return 0;
}

```

## 4. 类和对象

![](https://i.imgur.com/LmHOJYU.png)

### 4.1封装

#### 4.1.1封装的意义

![](https://i.imgur.com/kjy9NMW.png)

```c++
//
// Created by JSQ on 2021/6/30.
//设计一个圆类求周长
#include <iostream>
#include <string>
using namespace std;
const double PI = 3.14;
class Circle
{
    //访问权限
//公共权限
public:
    //属性
    // 半径
    int m_r;
    //行为
    // 获取圆的周长
    double calculateZC(){
        return 2 * PI *m_r;
    }
};
class Student
{
public:
    // 类中的属性和行为统一成为成员
    //属性 成员属性 成员变量
    //行为 成员函数 成员方法
    //属性
    string m_Name;
    int m_Id;
    //行为-显示
    void showStu(){
        cout<<"姓名:"<<m_Name<<" 学号："<<m_Id<<endl;
    }
    //给属性赋值
    void setName(string name){
        m_Name = name;
    }
    void setId(int id){
        m_Name = id;
    }
};
int main(){
    //创建对象
    Circle c1;
    //给对象赋值
    c1.m_r = 10;
    cout<<"周长："<<c1.calculateZC()<<endl;

    //封装实例
    Student s1;
//    s1.m_Name = "张三";
    s1.setName("张三");
    s1.m_Id = 1;
    s1.showStu();
    return 0;
}
```

![](https://i.imgur.com/yLylnoX.png)
```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>
#include <string>
using namespace std;
//三种访问权限
// 公共 成员 类内外都可访问
// 保护 成员 类内可以访问，类外不可以访问-子类可以访问
// 私有 成员 类内可以访问，类外不可以访问-子类不可以访问
class myPerson{
public:
    string m_Name;
    void Init(){
        //类内均可访问
        m_Name = "张三";
        m_Car = "保时捷";
        m_Pwd = 123456;
    }
protected:
    string m_Car;
private:
    int m_Pwd;
};
int main(){
    myPerson p1;
    p1.m_Name = "李四";
//    p1.m_Car = "奔驰";
//    p1.m_Pwd;
//    类外不可访问
    return 0;
}

```

#### 4.1.2 struct和class区别

**唯一区别在于：`默认的访问权限不同`**

- struct默认权限为公共
- class默认权限为私有

```c++
//
// Created by JSQ on 2021/6/30.
//
//- struct默认权限为公共
//- class默认权限为私有
#include <iostream>
#include <string>
using namespace std;
class C1_412{
    int m_A;//默认 private
};
struct C2_412{
    int m_A;//公共
};
int main(){
    C1_412 c1;
//    c1.m_A = 100;报错
    C2_412 c2;
    c2.m_A =100;//可访问

    return 0;
}
```

#### 4.1.3 成员属性设置为私有

![](https://i.imgur.com/OUeC2Wl.png)

```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>
#include <string>
using namespace std;
//成员属性设置为私有
// 1、可以自己控制读写权限
// 2、对于写可以检测数据的有效性

class Person413
{
private:
    //可读可写
    string m_Name;
    // 只读
    int m_Age;
    // 只写
    string m_Lover;
public:
    //姓名读写
    void setName(string name){
        m_Name = name;
    }
    string getName(){return m_Name;}
    //年龄只读
    int getAge(){
        m_Age = 0;//init
        return m_Age;
    }
    //情人只写
    void setLover(string lover){
        m_Lover = lover;
    }
};
int main(){
    Person413 p;
    p.setName("张三");
    cout<<"姓名："<<p.getName()<<endl;
    cout<<"年龄："<<p.getAge()<<endl;
    //设置情人
    p.setLover("BadGuys");
    return 0;
}

```

**练习案例1：设计立方体类**

![](https://i.imgur.com/UvxW960.png)

```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>
#include <string>
using namespace std;
//立方体类设计
// 1、创建立方体类
// 2、设计属性
// 3、设计行为获取立方体面积和体积
// 4、分别利用全局函数和成员函数判断两个立方体是否相等

class Cube41
{
private:
    //长宽高
    int m_L;
    int m_W;
    int m_H;
public:
    //设置和获取长宽高
    void setL(int L){m_L = L;}
    void setW(int W){m_W = W;}
    void setH(int H){m_H = H;}
    int getL(){return m_L;}
    int getW(){return m_W;}
    int getH(){return m_H;}
    //获取立方体面积和体积
    int calcul_s(){return 2*(m_L*m_W+m_H*m_W+m_L*m_H);}
    int calcul_v(){return m_L*m_H*m_W;}
    //利用成员函数判断两个立方体是否相等
    bool isSameByClass(Cube41 &c){
        if(c.getL() == m_L && c.getW() == m_W && c.getH() == m_H ){
            return true;
        }
        return false;
    }
};
//利用全局函数判断两个立方体是否相等
bool isSame_Cube(Cube41 &c1,Cube41 &c2){
    if(c1.getL() == c2.getL()&&c1.getW() == c2.getW()&&c1.getH() == c2.getH()){
        return true;
    }
    return false;
}
int main(){
    Cube41 c1;
    c1.setL(10);
    c1.setW(10);
    c1.setH(10);
    cout<<"c1的面积："<<c1.calcul_s()<<endl;
    cout<<"c1的体积："<<c1.calcul_v()<<endl;

    Cube41 c2;
    c2.setL(10);
    c2.setW(10);
    c2.setH(11);
    cout<<"c2的面积："<<c2.calcul_s()<<endl;
    cout<<"c2的体积："<<c2.calcul_v()<<endl;

    bool ret = isSame_Cube(c1,c2);
    if(ret){
        cout<<"c1和c2相等"<<endl;
    } else{
        cout<<"c1和c2不相等"<<endl;
    }
    ret = c1.isSameByClass(c2);
    if(ret){
        cout<<"成员函数判断：c1和c2相等"<<endl;
    } else{
        cout<<"成员函数判断：c1和c2不相等"<<endl;
    }
    return 0;
}

```



**设计案例二：点和圆的关系**（圆上、圆外、圆内）

![](https://i.imgur.com/1T2yoAC.png)

```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>
#include <string>
#include "MyPoint41.h"
#include "MyCircle41.h"
using namespace std;
//点和圆关系判断
//创建点类
class Point41{
private:
    int m_X;
    int m_Y;
public:
    void setX(int x){m_X = x;}
    int getX(){return m_X;}
    void setY(int y){m_Y = y;}
    int getY(){return m_Y;}
};
// 创建圆类
class Circle41
{
private:
    int m_R;
    //在类中可以让另一个类作为本类中的成员
    Point41 m_center;
public:
    void setR(int r){m_R = r;}
    int getR(){return m_R;}
    void setCenter(Point41 center){m_center = center;}
    Point41 getCenter(){return m_center;}
};
//判断点和圆的关系
//void isInCircle41(Circle41 &c, Point41 &p){
//    //计算距离
//    int dist = (c.getCenter().getX() - p.getX())*(c.getCenter().getX() - p.getX())+
//                (c.getCenter().getY() - p.getY())*(c.getCenter().getY() - p.getY());
//    //半径
//    int rDist = c.getR()*c.getR();
//    if(dist == rDist){
//        cout<<"点在圆上"<<endl;
//    }
//    else if(dist>rDist){
//        cout<<"点在圆外"<<endl;
//    } else{
//        cout<<"点在圆内"<<endl;
//    }
//}

void isInCircle41(MyCircle41 &c, MyPoint41 &p){
    //计算距离
    int dist = (c.getCenter().getX() - p.getX())*(c.getCenter().getX() - p.getX())+
               (c.getCenter().getY() - p.getY())*(c.getCenter().getY() - p.getY());
    //半径
    int rDist = c.getR()*c.getR();
    if(dist == rDist){
        cout<<"点在圆上"<<endl;
    }
    else if(dist>rDist){
        cout<<"点在圆外"<<endl;
    } else{
        cout<<"点在圆内"<<endl;
    }
}
int main(){
//    Circle41 c;
//    c.setR(10);
//    Point41 center;
//    center.setX(10);
//    center.setY(0);
//    c.setCenter(center);
//    Point41 p;
//    p.setX(10);
//    p.setY(9);
    MyCircle41 c;
    c.setR(10);
    MyPoint41 center;
    center.setX(10);
    center.setY(0);
    c.setCenter(center);
    MyPoint41 p;
    p.setX(10);
    p.setY(9);
    isInCircle41(c,p);
    return 0;
}

```



### 4.2 对象的初始化和清理

![](https://i.imgur.com/aU9JUNV.png)

#### 4.2.1构造函数和析构函数

![](https://i.imgur.com/rLuDTSn.png)

![](https://i.imgur.com/LEgpk5l.png)

```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>
#include <string>
using namespace std;
//对象的初始化和清理
// 1、构造函数 进行初始化操作
class Person421{
    //1.构造函数
    // 没有返回值 不用写void
    // 函数名与类名相同
    // 构造函数可以有参数，可以发生重载
    // 创建对象时，构造函数会自动调用，而且只调用一次
public:
    Person421(){
        cout<<"Person 构造函数的调用"<<endl;
    }
    //2.析构函数
    // 没有返回值 不写void
    // 函数名与类名相同，加~
    // 构造函数不可以有参数，不可以发生重载
    // 销毁对象时，析构函数会自动调用，而且只调用一次

    ~Person421(){
        cout<<"Person 析构函数的调用"<<endl;
    }
};
void test42object(){
    Person421 p;//栈上的数据
}
int main(){
//    test42object();
    Person421 p;
    return 0;
}

```

#### 4.2.2构造函数的分类及调用

![](https://i.imgur.com/lJtZSmp.png)

```c++
//
// Created by JSQ on 2021/6/30.
//
#include <iostream>
#include <string>
using namespace std;
//构造函数的分类及调用
// 分类：
// 按参数：无参构造和有参构造
// 按类型：普通构造 拷贝构造
class Person422{
    //1.构造函数
public:
    int age;
    Person422(){
        cout<<"Person 无参构造函数的调用"<<endl;
    }
    Person422(int a){
        age = a;
        cout<<"Person 有参构造函数的调用"<<endl;
    }
    //拷贝构造函数
    Person422(const Person422 &p){
        //将传入的人身上的所有属性进行拷贝
        age = p.age;
        cout<<"Person 拷贝构造函数的调用"<<endl;
    }
    //2.析构函数
    ~Person422(){
        cout<<"Person 析构函数的调用"<<endl;
    }
};
//调用
void test0422(){
    //1、括号法-默认构造函数不要加()
    // Person422 p1();会被视为函数的声明
//    Person422 p1;//Person 无参构造函数的调用(默认构造)
//    Person422 p2(10);//Person 有参构造函数的调用
//    Person422 p3(p2);//Person 拷贝构造函数的调用
//    cout<<"p2的年龄："<<p2.age<<endl;
//    cout<<"p3的年龄："<<p3.age<<endl;

    // 2、显示法-不要利用拷贝构造函数初始化匿名对象
    // Person422(p3);编译器会认为Person422(p3) == Person422 p3;对象声明-重定义
    Person422 p1;//默认无参构造
    Person422 p2 = Person422(10);//有参构造
    Person422 p3 = Person422(p2);//拷贝构造
    //Person422(10) 匿名对象 特点：当前行执行结束后系统会立即回收掉匿名对象
    // 3、隐式转换法
    Person422 p4 = 10;//有参构造 - Person422 p4 = Person422(10);
    Person422 p5 = p4;//拷贝构造 - Person422 p5 = Person422(p4);//拷贝构造
}
int main(){
    test0422();
//    Person421 p;
    return 0;
}
```
#### 4.2.3拷贝构造函数调用时机

![](https://i.imgur.com/lqZZbYs.png)

```c++
//
// Created by JSQ on 2021/7/1.
//
#include <iostream>
#include <string>
using namespace std;
//拷贝构造函数调用时机
// 1.使用一个已经创建完毕的对象来初始化一个新对象
// 2、值传递的方式给函数参数传值
// 3、值方式返回局部对象
class Person423{
    //1.构造函数
public:
    int age;
    Person423(){
        cout<<"Person 无参构造函数的调用"<<endl;
    }
    Person423(int a){
        age = a;
        cout<<"Person 有参构造函数的调用"<<endl;
    }
    //拷贝构造函数
    Person423(const Person423 &p){
        //将传入的人身上的所有属性进行拷贝
        age = p.age;
        cout<<"Person 拷贝构造函数的调用"<<endl;
    }
    //2.析构函数
    ~Person423(){
        cout<<"Person 析构函数的调用"<<endl;
    }
};
//调用
// 1.使用一个已经创建完毕的对象来初始化一个新对象
void test01(){
    Person423 p1(20);
    Person423 p2(p1);
}
// 2、值传递的方式给函数参数传值
void doWork(Person423 p){

}
void test02(){
    Person423 p;
    doWork(p);
}
// 3、值方式返回局部对象
Person423 doWork2(){
    Person423 p1;
    cout<<(int)&p1<<endl;
    return p1;
}
void test03(){
    Person423 p = doWork2();
    cout<<(int)&p<<endl;
}
int main(){
    test03();

    return 0;
}

```
#### 4.2.4构造函数调用规则

![](https://i.imgur.com/wmkSj3j.png)

```c++
//
// Created by JSQ on 2021/7/2.
//
#include <iostream>
#include <string>
using namespace std;
//构造函数的调用规则
// 1、创建一个类，C++编译器会给每一个类都添加至少三个函数
// 默认构造（空实现）
// 析构函数（空实现）
// 拷贝构造（值拷贝）
// 2、如果我们写了有参构造函数，编译器就不再提供默认构造，依然提供拷贝构造
// 3、如果我们写了拷贝构造函数，编译器就不再提供其他构造函数
class Person424{
public:
    int m_age;
//    Person424(){
//        cout<<"默认构造函数调用"<<endl;
//    }
//    Person424(int age){
//        m_age = age;
//        cout<<"有参构造函数调用"<<endl;
//    }
    Person424(const Person424 &p){
        m_age = p.m_age;
        cout<<"拷贝构造函数调用"<<endl;
    }
    ~Person424(){
        cout<<"析构函数调用"<<endl;
    }
};
//void test01424(){
//    Person424 p1;
//    p1.m_age = 18;
//    Person424 p2(p1);
//    cout<<"p1的年龄："<<p1.m_age<<endl;
//    cout<<"p2的年龄："<<p2.m_age<<endl;
//}
//void test02424() {
////    Person424 p1;
//    //当前自己写了有参构造，没写默认构造--报错
//    // 如果我们写了有参构造函数，编译器就不再提供默认构造，依然提供拷贝构造
////    p1.m_age = 18;
//    Person424 p1(12);
//    Person424 p2(p1);
//    cout << "p1的年龄：" << p1.m_age << endl;
//    cout << "p2的年龄：" << p2.m_age << endl;
//}
void test03424() {
    Person424 p1;
    //当前自己写了拷贝构造，没写其他构造--报错
    // 如果我们写了拷贝构造函数，编译器就不再提供其他构造函数
    Person424 p2(p1);
    cout << "p1的年龄：" << p1.m_age << endl;
    cout << "p2的年龄：" << p2.m_age << endl;
}

int main(){
    test03424();
    return 0;
}
```
#### 4.2.5深拷贝与浅拷贝

- 浅拷贝：简单的赋值拷贝操作
- 深拷贝：在堆区重新申请空间，进行拷贝操作

```c++
//
// Created by JSQ on 2021/7/2.
//
#include <iostream>
#include <string>
using namespace std;
//深拷贝与浅拷贝
class Person425{
public:
    int m_age;
    int *m_Height;//开辟在堆区的参数
    Person425(){
        cout<<"默认构造函数调用"<<endl;
    }
    Person425(int age, int height){
        m_age = age;
        m_Height = new int(height);
        cout<<"有参构造函数调用"<<endl;
    }
    //自己实现拷贝构造函数 解决浅拷贝带来的问题
    Person425(const Person425 &p){
        m_age = p.m_age;
//        m_Height = p.m_Height;编译器默认实现
        m_Height = new int(*p.m_Height);
        cout<<"拷贝构造函数调用"<<endl;
    }
    ~Person425(){
        //析构代码，将堆区开辟数据做释放操作
        if(m_Height != NULL){
            delete m_Height;
            m_Height = NULL;
        }
//        delete m_Height;
//        m_Height = NULL;
        cout<<"析构函数调用"<<endl;
    }
};
void test01425(){
    Person425 p1(18,160);
    cout << "p1的年龄：" << p1.m_age << "，p1的身高：" <<*p1.m_Height << endl;
    cout << "p1的年龄：" << (int)&p1.m_age << "，p1的身高：" <<p1.m_Height << endl;
    Person425 p2(p1);//浅拷贝--带来的问题是堆区内存重复释放
    //浅拷贝的问题利用深拷贝解决
    cout << "p2的年龄：" << p2.m_age << "，p2的身高：" <<*p2.m_Height <<endl;
    cout << "p2的年龄：" << (int)&p2.m_age << "，p2的身高：" <<p2.m_Height <<endl;
}
int main(){
    test01425();
    return 0;
}

```
> 总结：如果属性有在堆区开辟的，一定要自己提供拷贝构造函数，放置浅拷贝带来的问题

#### 4.2.6初始化列表

**作用：**初始化属性

```c++
//
// Created by JSQ on 2021/7/2.
//
#include <iostream>
#include <string>
using namespace std;
//初始化列表
class Person426{
public:
    //传统初始化操作
//    Person426(int a,int b,int c){
//        A = a;
//        B = b;
//        C = c;
//    }
    //初始化列表初始化属性
    Person426(int a,int b,int c):A(a),B(b),C(c){}
    int A;
    int B;
    int C;

};

void test0425(){
//    Person426 p(10,20,30);
//    Person426 p;
    Person426 p(20,10,30);
    cout<<"A="<<p.A<<endl;
    cout<<"B="<<p.B<<endl;
    cout<<"C="<<p.C<<endl;
}
int main(){
    test0425();
    return 0;
}
```
#### 4.2.7类对象作为类成员

C++类中的成员可以是另一个类的对象，我们称该成员为对象成员

例子：

```c++
class A{}
class B{
 A a;
}
```

B类有对象A作为成员，那么创建B对象时，A和B的构造和析构顺序如何？

```c++
//
// Created by JSQ on 2021/7/2.
//
#include <iostream>
#include <string>
using namespace std;
//类对象作为类成员
// 当其他类对象作为本类成员，构造时候先构造类对象，再构造自身 
// 析构的顺序与构造相反
class Phone427{
public:
    Phone427(string name){
        pName = name;
        cout<<"Phone的构造函数调用"<<endl;
    }
    string pName;
    ~Phone427(){
        cout<<"Phone的析构函数调用"<<endl;
    }
};
class Person427{
public:
    Person427(string hname,string pname):name(hname), phone(pname){
        cout<<"Person的构造函数调用"<<endl;
    }
    string name;
    Phone427 phone;
    ~Person427(){
        cout<<"Person的析构函数调用"<<endl;
    }
};
void test1427(){
    Person427 p("张三","苹果");
    cout<< p.name <<" "<< p.phone.pName <<endl;
}

int main(){
    test1427();
    return 0;
}

```
#### 4.2.8静态成员

![](https://i.imgur.com/UjuTBxa.png)

```c++
//
// Created by JSQ on 2021/7/3.
//
#include <iostream>
#include <string>
using namespace std;
//静态成员变量

class Person428{
public:
    static int A;
    //1.所有对象都共享同一份数据
    // 2.编译阶段就分配内存
    // 3.类内声明，类外初始化操作
private:
    static int B;
};
//类外初始化
int Person428::A = 100;
int Person428::B = 100;
void test14281(){
    Person428 p;
    cout<<p.A<<endl;
    Person428 p2;
    p2.A = 200;//共享同一份数据
    cout<<p.A<<endl;
//    没有初始化-报错
}
void test24281(){
    //静态成员变量 不属于某个对象上，所有对象都共享同一份数据
    // 静态成员变量两种访问方式
    // 1、通过对象进行访问
    Person428 p;
    cout<<p.A<<endl;
    //2、通过类名进行访问
    cout<<Person428::A<<endl;
//    cout<<Person428::B<<endl; 类外访问不到私有静态成员变量
}

int main(){
    test24281();
    return 0;
}

```

```c++
//
// Created by JSQ on 2021/7/3.
//
#include <iostream>
#include <string>
using namespace std;
//静态成员函数
// 所有对象共享同一个函数
// 静态成员函数只能访问静态成员变量

class Person429{
public:
    static int A;
    int B;
    static void func429(){
        A = 300;//静态成员函数可以访问 静态成员变量
//        B = 200;//静态成员函数不可以访问 非静态成员变量，无法区分到底是哪个对象的属性
        cout<<"静态函数调用"<<endl;
    }
    //静态成员函数也是有访问权限的
private:
    static void func2429(){
        cout<<"私有静态函数调用"<<endl;
    }
};
int Person429::A = 100;
void test1429(){
    //1.通过对象访问
    Person429 p;
    p.func429();
    // 2.通过类名访问
    Person429::func429();

//    Person429::func2429();//类外访问不到私有静态成员函数
}

int main429(){
    test1429();
    return 0;
}

```

### 4.3C++对象模型和this指针

#### 4.3.1成员变量和成员函数分开存储

![](https://i.imgur.com/zKPaq1D.png)

```c++
//
// Created by JSQ on 2021/7/3.
//
#include <iostream>
#include <string>
using namespace std;
//成员变量和成员函数分开存储
class Person01{
    int a; //非静态成员变量 属于类的对象上
    static int A;//静态成员变量 不属于类的对象上
    void func0101(){//非静态成员函数 不属于类的对象上

    }
    static void func0102(){}//静态成员函数 不属于类的对象上
};
int Person01::A = 100;
void test0101(){
    Person01 p;
    //空对象占用内存空间为：1
    // C++编译器会给每个空对象也分配一个字节空间，为了区分空对象占内存的位置 ，
    // 每个空对象也应该有一个独一无二的内存地址
    cout<<"size of p="<<sizeof(p)<<endl;
}
void test0201(){
    Person01 p;//有一个非静态成员变量-增加静态成员变量不变
    cout<<"size of p="<<sizeof(p)<<endl;
}
int main(){
    test0201();
    return 0;
}
```
#### 4.3.2this指针概念

![](https://i.imgur.com/JT513qr.png)

```c++
//
// Created by JSQ on 2021/7/3.
//
#include <iostream>
#include <string>
using namespace std;

class Person02{
public:
    int age;
//    Person02(int age){
//        age = age;
//    }名称冲突-会导致后续乱码，系统默认三者相同
    Person02(int age){
        //this指针指向的是 被调用的成员函数所属对象
        this->age = age;
    }
    //Person02 PersonAddage(Person02 &p)
    //以值的方式返回，那么在每一次调用之后都会重新创建一个新的对象
    //以引用的方式返回，就不会创建，一直都会是原有的对象
    Person02& PersonAddage(Person02 &p){
        this->age += p.age;
        //this指向p2的指针，而*this指向的就是p2这个对象的本体
        return *this;
    }
};
//1 解决名称冲突
void test0102(){
    Person02 p1(18);
    cout<<p1.age<<endl;
}
// 2 返回对象本身用*this
void test0202(){
    Person02 p1(10);
    Person02 p2(10);
    //链式编程思想
    p2.PersonAddage(p1).PersonAddage(p1);
    cout<<p1.age<<endl;
    cout<<p2.age<<endl;

}
int main(){
    test0202();
    return 0;
}
```
#### 4.3.3空指针访问成员函数

![](https://i.imgur.com/7nYGqRY.png)

```c++
//
// Created by JSQ on 2021/7/3.
//
#include <iostream>
#include <string>
using namespace std;

class Person03{
public:
    int age;
    void showClassName(){
        cout<<"person class"<<endl;
    }
    void showPersonAge(){
        if(this == NULL){//提高系统健壮性
            return;
        }
        cout<<"age:"<<age<<endl;
    }
};

void test0103(){
    Person03 *p = NULL;
    p->showClassName();
    p->showPersonAge();//报错-因为传入指针为NULL
}

void test0203(){
//    Person03 p1(10);
//    Person03 p2(10);
//
//    cout<<p1.age<<endl;
//    cout<<p2.age<<endl;

}
int main(){
    test0103();
    return 0;
}


```
#### 4.3.4const修饰成员函数

![](https://i.imgur.com/NGnth7I.png)

```c++
//
// Created by JSQ on 2021/7/3.
//
#include <iostream>
#include <string>
using namespace std;
//常函数
class Person04{
public:
    int A=100;
    mutable int B;//特殊变量，即使在常函数中也可以修改这个值，加关键字mutable
    //this指针的本质是指针常量 指针的指向不可修改
    //this = NULL;报错
    //而在函数后加const相当于 const Person03 * const this;
    //加了之后成员变量也不可修改
    //在成员函数后面加const，修饰的是this指针，让指针指向的值也不可以修改
    void func04(){

    }
    void showPersonAge() const
    {
        this->B = 100;
//        this->age = 100;
    }
};

//常对象
void test0104(){
    const Person04 p;//在对象前加const，变成常对象
//    p.A = 100;
    p.B = 1000;//特殊值，在常对象下也可以修改
    //常对象只能调用常函数
    p.showPersonAge();
//    p.func04();常对象不可以调用普通成员函数，因为普通成员函数可以修改属性
}
int main(){
    test0104();
    return 0;
}
```

### 4.4友元

![](https://i.imgur.com/Qb70OSg.png)

#### 4.4.1全局函数做友元
```c++
//
// Created by JSQ on 2021/7/4.
//
#include <iostream>
#include <string>
using namespace std;
class Building{
    friend void goodGay(Building *building);
    //全局函数作为友元可以访问类中私有成员
public:
    string sitting_room;
private:
    string bedroom;
public:
    Building(){
        sitting_room = "客厅";
        bedroom = "卧室";
    }
};
//全局函数
void goodGay(Building *building){
    cout<<"全局函数正在访问"<<building->sitting_room<<endl;
    cout<<"全局函数正在访问"<<building->bedroom<<endl;
}
void test0101(){
    Building building;
    goodGay(&building);
}
int main(){
    test0101();
    return 0;
}
```
#### 4.4.2类做友元
```c++
//
// Created by JSQ on 2021/7/4.
//
#include <iostream>
#include <string>
using namespace std;
//类做友元
class Building02;
class GoodGay{
public:
    void visit();//参观函数 访问Building中的属性
    Building02 *building;
    GoodGay();
};
class Building02{
    friend class GoodGay;//类做友元 可以访问本类中的私有成员
private:
    string bedroom;
public:
    string settingroom;
public:
    Building02();
};
//类外写成员函数
Building02::Building02() {
    settingroom = "客厅";
    bedroom = "卧室";
}
GoodGay::GoodGay() {
    building = new Building02;
}
void GoodGay::visit() {
    cout<<"好基友正在访问:"<<building->settingroom<<endl;
    cout<<"好基友正在访问:"<<building->bedroom<<endl;
}
void test0201(){
    GoodGay gg;
    gg.visit();
}
int main(){
    test0201();
    return 0;
}
```
#### 4.4.3成员函数做友元

```c++
//
// Created by JSQ on 2021/7/4.
//
#include <iostream>
#include <string>
using namespace std;
//成员函数做友元
class Building02;
class GoodGay{
public:
    void visit();//参观函数 访问Building中的私有成员
    void visit2();//参观函数 不可以访问Building中的私有成员
    Building02 *building;
    GoodGay();
};
class Building02{
    friend void GoodGay::visit();//成员函数做友元 可以访问本类中的私有成员
private:
    string bedroom;
public:
    string settingroom;
public:
    Building02();
};
//类外写成员函数
Building02::Building02() {
    settingroom = "客厅";
    bedroom = "卧室";
}
GoodGay::GoodGay() {
    building = new Building02;
}
void GoodGay::visit() {
    cout<<"visit 好基友正在访问:"<<building->settingroom<<endl;
    cout<<"visit 好基友正在访问:"<<building->bedroom<<endl;
}
void GoodGay::visit2() {
    cout<<"visit2 好基友正在访问:"<<building->settingroom<<endl;
//    cout<<"visit2 好基友正在访问:"<<building->bedroom<<endl;报错
}
void test0201(){
    GoodGay gg;
    gg.visit();
    gg.visit2();
}
int main(){
    test0201();
    return 0;
}
```

### 4.5运算符重载

运算符重载：对已有的运算符重新定义，赋予其另一种功能，以适应不同的数据类型

#### 4.5.1加号运算符重载

- 实现两个自定义数据类型的运算

```c++
//
// Created by JSQ on 2021/7/4.
//
#include <iostream>
#include <string>
using namespace std;
//加号运算符重载
class Person01{
public:
    int a;
    int b;
    //1.成员函数重载+
//    Person01 operator+(Person01 &p){
//        Person01 temp;
//        temp.a = this->a + p.a;
//        temp.b = this->b + p.b;
//        return temp;
//    }
};
//2.全局函数重载+
Person01 operator+(Person01 &p1,Person01 &p2){
    Person01 temp;
    temp.a = p1.a + p2.a;
    temp.b = p1.b + p2.b;
    return temp;
}
//函数重载版本
Person01 operator+(Person01 &p1,int b){
    Person01 temp;
    temp.a = p1.a + b;
    temp.b = p1.b + b;
    return temp;
}
void test0101(){
    Person01 p1;
    Person01 p2;
    p1.a = 10;
    p1.b = 20;
    p2.a = 10;
    p2.b = 20;

    //成员函数重载本质调用
//    Person01 p3 = p1.operator+(p2);
    //全局函数重载本质调用
//    Person01 p3 = operator+(p1,p2);
    Person01 p3 = p1+p2;
    //运算符重载也可以发生重载
    Person01 p4 = p1+10;
    cout<<p3.a<<","<<p3.b<<endl;
    cout<<p4.a<<","<<p4.b<<endl;
}

int main(){
    test0101();
    return 0;
}
```
> 总结：
>
> - 对于内置的数据类型的表达式的运算符是不可能改变的
> - 不要滥用运算符重载

#### 4.5.2左移运算符重载

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//左移运算符重载
class Person02{
friend ostream& operator<<(ostream &cout, Person02 &p);

public:
    Person02(int A, int B){
        a = A;
        b = B;
    }
private:
    int a;
    int b;
    //利用成员函数重载 <<
    // 不会利用成员函数重载<<运算符，因为无法实现cout在左侧

};
//只能利用全局函数重载<<
ostream& operator<<(ostream &cout, Person02 &p){
    cout<<p.a<<","<<p.b<<endl;
    return cout;
}


void test0201(){
    Person02 p1(10,20);

    cout<<p1;
    //链式编程
    cout<<p1<<"chain"<<endl;

}

int main(){
    test0201();
    return 0;
}
```
#### 4.5.3递增运算符重载
```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//重载递增运算符
class MyInteger{
friend ostream & operator<<(ostream &cout, MyInteger p);
public:
    MyInteger(int n){
        num = n;
    }
    //重载 前置++运算符
    MyInteger& operator++(){
        //先进行++运算
        this->num ++;
        //再将自身做返回
        return *this;
    }
    //重载 后置++运算符
    // （int）代表占位参数，用于区分前置和后置递增
    MyInteger operator++(int){
        //先 记录当前结果
        MyInteger temp = *this;
        //后 递增
        num ++;
        // 将记录结果返回
        return temp;
    }
private:
    int num;

};
ostream & operator<<(ostream &cout, MyInteger p)
{
    cout<<p.num;
    return cout;
}
void test0301(){
    MyInteger myint(10);
    //前置递增 只能返回引用，为的是一直对一个数据进行操作；
    // 值返回的话会重新创建新的对象
    cout<<++(++myint)<<endl;
    cout<<myint<<endl;
}
void test0302(){
    MyInteger myint(10);
    //前置递增 只能返回引用，为的是一直对一个数据进行操作；
    // 值返回的话会重新创建新的对象
    cout<< myint++ <<endl;
    cout<< myint<<endl;
}
int main(){
    test0302();
    return 0;
}
```
#### 4.5.4赋值运算符重载

![](https://i.imgur.com/HFLLtTt.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//赋值运算符重载
class Person04{
public:
    Person04(int a){
        age = new int(a);
    }
    ~Person04(){
        if(age!=NULL){
            delete age;
            age = NULL;
        }
    }
   Person04& operator=(Person04 &p){
        //编译器提供的是浅拷贝
        // age = p.age
        // 应该先判断当前堆区是否有属性，如有先释放再进行深拷贝
        if(age!=NULL){
            delete age;
            age = NULL;
        }
        age = new int(*p.age);
        return *this;
    }
    int *age;
};
void test0401(){
    Person04 p1(18);
    Person04 p2(20);
    Person04 p3(30);
    cout<<*p1.age<<endl;
    cout<<*p2.age<<endl;
    p3 = p2 = p1;//赋值操作-链式编程
    cout<<*p2.age<<endl;
    cout<<*p3.age<<endl;
    //vs中会由于同一个内存多次释放而报错
    //需要重载赋值操作
}
int main(){
    test0401();
    return 0;
}
```
#### 4.5.5关系运算符重载
```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//赋值运算符重载
class Person05{
public:
    Person05(string n,int a){
        age = a;
        name = n;
    }
    //重载==
    bool operator==(Person05 &p){
        if(this->name == p.name && this->age == p.age){
            return true;
        } else{
            return false;
        }
    }
    bool operator!=(Person05 &p){
        if(this->name == p.name && this->age == p.age){
            return false;
        } else{
            return true;
        }
    }
    string name;
    int age;
};
void test0501(){
    Person05 p1("Jerry",18);
    Person05 p2("Jerry",18);
    if(p1==p2){
        cout<<"=="<<endl;
    }else{cout<<"!="<<endl;}
    if(p1!=p2){
        cout<<"!="<<endl;
    }else{cout<<"=="<<endl;}

}
int main(){
    test0501();
    return 0;
}


```
#### 4.5.6函数调用运算符重载

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//仿函数
//函数调用运算符重载
class MyPrint{
public:
    //重载函数调用运算符
    void operator()(string test){
        cout<<test<<endl;
    }

};
void test0601(){
    MyPrint myprint;
    myprint("hello world");//由于使用起来非常类似于函数调用，因此称为仿函数
}
//仿函数非常灵活，没有固定的写法
//加法类
class MyAdd{
public:
    int operator()(int num1,int num2){
        return num1 + num2;
    }
};
void test0602(){
    MyAdd myadd;
    cout<<myadd(100,200)<<endl;
    //匿名函数对象
    cout<<MyAdd()(100,300)<<endl;
}
int main(){
    test0601();
    test0602();
    return 0;
}
```

### 4.6继承

**继承是面向对象的三大特性之一**

![](https://i.imgur.com/D1ed6bh.png)

#### 4.6.1 继承的基本语法

![](https://i.imgur.com/xYvl7aC.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//普通实现
//java页面
//class Java{
//public:
//    void header(){
//        cout<<"首页。。。。"<<endl;
//    }
//    void footer(){
//        cout<<"帮助重心。。。。"<<endl;
//    }
//    void left(){
//        cout<<"java、python。。。。"<<endl;
//    }
//    void content(){
//        cout<<"Java学科内容"<<endl;
//    }
//};
////python页面
//class Python{
//public:
//    void header(){
//        cout<<"首页。。。。"<<endl;
//    }
//    void footer(){
//        cout<<"帮助重心。。。。"<<endl;
//    }
//    void left(){
//        cout<<"java、python。。。。"<<endl;
//    }
//    void content(){
//        cout<<"Python学科内容"<<endl;
//    }
//};

//继承实现页面
// 继承的好处：减少重复代码 
// 语法： class 子类：继承方式 父类 
// 子类也成为派生类 
// 父类也成为基类

class BasePage01{
public:
    void header(){
        cout<<"首页。。。。"<<endl;
    }
    void footer(){
        cout<<"帮助重心。。。。"<<endl;
    }
    void left(){
        cout<<"java、python。。。。"<<endl;
    }
};
//java页面
class Java: public BasePage01{
public:
    void content(){
        cout<<"java学科内容"<<endl;
    }
};
//python页面
class Python: public BasePage01{
public:
    void content(){
        cout<<"python学科内容"<<endl;
    }
};
void test0101(){
    cout<<"Java下载视频页面如下："<<endl;
    Java ja;
    ja.header();
    ja.footer();
    ja.left();
    ja.content();
    cout<<"---------------------------"<<endl;
    cout<<"Python下载视频页面如下："<<endl;
    Python py;
    py.header();
    py.footer();
    py.left();
    py.content();
}
int main(){
    test0101();
    return 0;
}

```
#### 4.6.2继承方式

![](https://i.imgur.com/HuQzS97.png)

![](https://i.imgur.com/4MebxBM.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//继承方式：
// 公共继承
class  Base1{
public:
    int a;
protected:
    int b;
private:
    int c;
};
class Son1: public Base1{
public:
    void func(){
        a = 10;//父类中的公共成员 到了子类依旧是公共权限
        b = 10;//父类中的保护成员 到了子类依旧是保护权限
//        c = 10;//父类中的私有成员 子类无法访问
    }
};
void test0201(){
    Son1 s1;
    s1.a = 100;
//    s1.b = 100;保护成员，类外不可访问
}
// 保护继承
class Son2:protected  Base1{
public:
    void func(){
        a = 10;//父类中的公共成员 到了子类变为保护权限
        b = 10;//父类中的保护成员 到了子类依旧是保护权限
//        c = 10;//父类中的私有成员 子类无法访问
    }
};
void test0202(){
    Son2 s1;
//    s1.a = 100;//Son2中该变量变为保护成员，类外不可访问
//    s1.b = 100;//保护成员，类外不可访问
}
// 私有继承
class Son3: private  Base1{
public:
    void func(){
        a = 10;//父类中的公共成员 到了子类变为私有权限
        b = 10;//父类中的保护成员 到了子类变为私有权限
//        c = 10;//父类中的私有成员 子类无法访问
    }
};
void test0203(){
    Son3 s1;
//    s1.a = 100;//Son3中该变量变为私有成员，类外不可访问
//    s1.b = 100;//私有成员，类外不可访问
}
class GrandSon3:public Son3{
public:
    void func(){
//        a = 1000;//父类中的私有成员 子类无法访问
//        b = 10000;//父类中的私有成员 子类无法访问
//        c = 10;//父类中的私有成员 子类无法访问
    }
};
int main(){

    return 0;
}
```
#### 4.6.3继承中的对象模型

从父类继承过来的成员，哪些属于子类对象中？

- 都会属于子类

![](https://i.imgur.com/DgKY08O.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//继承中的对象模型
class Base{
public:
    int a;
protected:
    int b;
private:
    int c;
};
class Son:public Base{
public:
    int d;
};
//利用开发人员命令提示工具查看对象模型
//查看命令
// cl /d1 reportSingleClassLayout类名 文件名
void test0301(){
    //16
    //父类中的所有非静态成员属性都会被子类继承下去
    // 父类中私有成员属性是被 编译器给隐藏了，因此无法访问，但确实被继承下去了
    cout<<"size of Son="<< sizeof(Son)<<endl;
}
int main(){
    test0301();
    return 0;
}
```
#### 4.6.4继承中的构造和析构顺序

![](https://i.imgur.com/YuoEsP9.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//继承中的构造和析构的顺序
class Base04{
public:
    Base04(){
        cout<<"Class Base struct"<<endl;
    }
    ~Base04(){
        cout<<"Class Base deconstruct"<<endl;
    }
};
class Son04:public Base04{
public:
    Son04(){
        cout<<"Class Son struct"<<endl;
    }
    ~Son04(){
        cout<<"Class Son deconstruct"<<endl;
    }
};
void test0401(){
    Son04 s;
    //先构造父类再构造子类
    //先析构子类再析构父类，与构造相反
}
int main(){
    test0401();
    return 0;
}
```
![](https://i.imgur.com/Scf0Onz.png)

#### 4.6.5继承同名成员处理方式

![](https://i.imgur.com/Ml0jhyx.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//继承中同名成员处理
class Base05{
public:
    Base05(){
        a = 10;
    }
    void func(){
        cout<<"func of Base"<<endl;
    }
    void func(int a){
        cout<<"func(int a) of Base"<<endl;
    }
    int a;
};
class Son05:public Base05{
public:
    Son05(){
        a = 30;
    }
    void func(){
        cout<<"func of Son"<<endl;
    }
    int a;
};
//同名成员变量
void test0501(){
    Son05 s;
    cout<<" Son of a="<<s.a<<endl;
    //如果通过类对象访问到 父类中同名成员需要加作用域
    cout<<" Base of a="<<s.Base05::a<<endl;
}
//同名成员函数
void test0502(){
    Son05 s;
    s.func();
    s.Base05::func();
    //如果子类中出现和父类同名的成员函数，
    // 子类的童名成员会隐藏掉父类中所有同名成员函数
//    s.func(100);//重载也不能直接调用也需要加作用域
    s.Base05::func(100);
}
int main(){
    test0502();
    return 0;
}
```
![](https://i.imgur.com/PDdsHJQ.png)

#### 4.6.6继承同名静态成员处理方式

![](https://i.imgur.com/Ek6AWO6.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//继承同名静态成员
class Base06{
public:
    static void func(){
        cout<<"func of Base"<<endl;
    }
    static void func(int a){
        cout<<"func(int a) of Base"<<endl;
    }
    static int a;
};
//静态成员需要类外初始化
int Base06::a = 100;
class Son06:public Base06{
public:
    static void func(){
        cout<<"func of Son"<<endl;
    }
    static int a;
};
int Son06::a = 200;
//同名静态成员属性
void test0601(){
    //1.通过对象访问
    cout<<"Through object to get"<<endl;
    Son06 s;
    cout<<" Son of a="<<s.a<<endl;
    //如果通过类对象访问到 父类中同名成员需要加作用域
    cout<<" Base of a="<<s.Base06::a<<endl;
    //2.通过类名访问
    cout<<"Through class name to get"<<endl;
    cout<<" Son of a="<<Son06::a<<endl;
    //第一个::代表通过类名方式访问 第二个::代表访问父类作用域下属性
    cout<<" Base of a="<<Son06::Base06::a<<endl;
}
//同名成员函数
void test0602() {
    //1.通过对象访问
    Son06 s;
    s.func();
    s.Base06::func();
    s.Base06::func(100);
    //2.通过类名访问
    Son06::func();
    Son06::Base06::func();
    Son06::Base06::func(100);
}
int main(){
    test0602();
    return 0;
}
```
#### 4.6.7多继承语法

![](https://i.imgur.com/YHok1RI.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//多继承语法
class Base07{
public:
    Base07(){
        a = 20;
    }
    int a;
};
class Base071{
public:
    Base071(){
        a = 30;
    }
    int a;
};
class Son07:public Base07,public Base071{
public:
    Son07(){
        c = 40;
        d = 50;
    }
    int c,d;
};
void test0701(){
    Son07 s;
    cout<< sizeof(Son07)<<endl;
    //当父类中出现同名成员，需要加作用域
    cout<<s.Base07::a<<endl;
    cout<<s.Base071::a<<endl;
}
int main(){
    test0701();
    return 0;
}
```
#### 4.6.8菱形继承

![](https://i.imgur.com/Cx4QBIr.png)

![](https://i.imgur.com/7EkjFR8.png)

**虚基类、虚继承原理：**

![](D:\JYH\Jobs\C++_learning\CppKernel\Mynotes.assets\mpHB8a5.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
using namespace std;
//动物类
class Animal{
public:
    int age;
};
//利用虚继承 解决菱形继承的问题，加上virtual
//羊类
//当前Animal成为虚基类
class Sheep:virtual public Animal{};
//驼类
class Camel:virtual public Animal{};
// 羊驼类
class Alpaca:public Sheep,public Camel{};
void test0801(){
    Alpaca s;
    //当出现菱形继承，有两个父类拥有相同的数据，需要加以作用域区分
    s.Sheep::age = 19;
    s.Camel::age = 30;
    cout<<s.Sheep::age<<endl;
    cout<<s.Camel::age<<endl;
    cout<<s.age<<endl;
    //但菱形继承导致数据有两份，但只需要一份，出现资源浪费
    // 虚继承解决
}
int main(){
    test0801();
    return 0;
}
```

### 4.7多态

![](D:\JYH\Jobs\C++_learning\CppKernel\Mynotes.assets\YJWufF9.png)

#### 4.7.1多态的基本概念
```c++
#include <iostream>
#include <string>
using namespace std;
//多态
class Animal{
public:
    //虚函数
    virtual void speak(){
        cout<<"animal speak"<<endl;
    }
};
class Cat:public Animal{
public:
    void speak(){
        cout<<"cat speak"<<endl;
    }
};
class Dog:public Animal{
public:
    void speak(){
        cout<<"Dog speak"<<endl;
    }
};
//地址早绑定 在编译阶段确定函数地址
// 如果想执行子类，这个函数地址不能提前绑定，需要在运行阶段进行绑定，地址晚绑定
// 利用虚函数实现

//动态多态满足条件
// 1.继承关系
// 2.子类重写（函数返回值类型 函数名 参数列表 完全相同）父类中的虚函数
//
// 动态多态的使用
// 父类的指针或者引用 执行子类对象

void doSpeak(Animal &animal){//Animal &animal = cat;
    animal.speak();
}
void test0101(){
    Cat cat;
    doSpeak(cat);
    Dog dog;
    doSpeak(dog);
}
int main(){
    test0101();
    return 0;
}
```
#### 4.7.2多态案例-计算器类

- 代码组织结构清晰
- 可读性强
- 利于前期和后期的拓展以及维护

```c++
//
// Created by JSQ on 2021/7/6.
//
#include <iostream>
#include <string>
using namespace std;
//分别利用普通写法和多态技术实现计算器
//普通写法
class Calculator{
public:
    int getResult(string oper){
        if(oper=="+"){
            return num1 + num2;
        } else if(oper=="-"){
            return num1 - num2;
        }else if(oper=="*"){
            return num1 * num2;
        }
        //如果想扩展新的功能，需要修改源码
        // 在真实开发中，提倡开闭原则
        // 开闭原则：对扩展进行开放，对修改进行关闭

    }
    int num1;
    int num2;
};
void test0201(){
    Calculator c;
    c.num1 = 10;
    c.num2 = 10;
    cout<<c.num1<<"+"<<c.num2<<"="<<c.getResult("+")<<endl;
    cout<<c.num1<<"-"<<c.num2<<"="<<c.getResult("-")<<endl;
    cout<<c.num1<<"*"<<c.num2<<"="<<c.getResult("*")<<endl;
}
//利用多态实现计算器
// 多态的好处：
// 1.组织结构清晰
// 2.可读性强
// 3.对于前期和后期扩展以及维护性高
//实现计算器抽象类
class AbsCalculator{
public:
    virtual int getResult(){
        return 0;
    }
    int num1;
    int num2;
};
class AddCalculator:public AbsCalculator{
public:
    int getResult(){
        return num1 + num2;
    }
};
class SubCalculator:public AbsCalculator{
public:
    int getResult(){
        return num1 - num2;
    }
};
class MulCalculator:public AbsCalculator{
public:
    int getResult(){
        return num1 * num2;
    }
};
void test0202(){
    //多态的使用条件：父类指针或者引用指向子类对象

    AbsCalculator *abc = new AddCalculator;//创建一个加法计算器对象
    abc->num1 = 10;
    abc->num2 = 10;
    cout<<abc->num1<<"+"<<abc->num2<<"="<< abc->getResult() <<endl;
    //用完销毁
    delete abc;
    //减法
    abc = new SubCalculator;
    abc->num1 = 10;
    abc->num2 = 10;
    cout<<abc->num1<<"-"<<abc->num2<<"="<< abc->getResult() <<endl;
    //用完销毁
    delete abc;
    //乘法
    abc = new MulCalculator;
    abc->num1 = 10;
    abc->num2 = 10;
    cout<<abc->num1<<"*"<<abc->num2<<"="<< abc->getResult() <<endl;
    //用完销毁
    delete abc;
}
int main02(){
    test0202();
    return 0;
}
```
#### 4.7.3纯虚函数类和抽象类

![](https://i.imgur.com/p61k6yN.png)

```c++
//
// Created by JSQ on 2021/7/6.
//
#include <iostream>
#include <string>
using namespace std;
//纯虚函数和抽象类
class Base{
public:
    //纯虚函数
    // 只要有一个纯虚函数，这个类就称为抽象类
    // 抽象类特点：
    // 1.无法实例化对象
    // 2.子类必须重写父类中的纯虚函数，否则也算抽象类
    virtual void func() = 0;
};
class Son:public Base{
public:
    virtual void func() {
        cout<<"func()"<<endl;
    }
};
void test0301(){
    //抽象类是无法实例化对象
//    Base b;报错
//    new Base;报错
//    Son s;//2.子类必须重写父类中的纯虚函数，否则也算抽象类
    Base *base = new Son;
    base->func();
}
int main(){
    test0301();
    return 0;
}
```
#### 4.7.4多态案例-制作饮品

![](https://i.imgur.com/F0Ot6Mf.png)

```c++
//
// Created by JSQ on 2021/7/6.
//
#include <iostream>
#include <string>
using namespace std;
//多态案例-制作饮品
class AbsDrinking{
public:
    virtual void Boil() = 0;
    virtual void Brew() = 0;
    virtual void PourInCup() = 0;
    virtual void PutSth() = 0;
    void makeDrink() {
        Boil();
        Brew();
        PourInCup();
        PutSth();
    }
};
//制作咖啡
class Caffe:public AbsDrinking{
    virtual void Boil() {
        cout<<"煮农夫山泉"<<endl;
    }
    virtual void Brew() {
        cout<<"冲泡咖啡"<<endl;
    }
    virtual void PourInCup() {
        cout<<"倒入马克杯中"<<endl;
    }
    virtual void PutSth() {
        cout<<"加入糖和牛奶"<<endl;
    }
};
class Tea:public AbsDrinking{
    virtual void Boil() {
        cout<<"煮矿泉水"<<endl;
    }
    virtual void Brew() {
        cout<<"冲泡茶叶"<<endl;
    }
    virtual void PourInCup() {
        cout<<"倒入茶杯中"<<endl;
    }
    virtual void PutSth() {
        cout<<"加入枸杞"<<endl;
    }
};
void doWork(AbsDrinking *abs){
    abs->makeDrink();
    delete abs;//释放
}
void test0401(){
    doWork(new Caffe);
    cout<<"--------------"<<endl;
    doWork(new Tea);
}
int main(){
    test0401();
    return 0;
}
```
#### 4.7.5虚析构和纯虚析构

![](D:\JYH\Jobs\C++_learning\CppKernel\Mynotes.assets\YeIRtFk.png)

```c++
//
// Created by JSQ on 2021/7/6.
//
#include <iostream>
#include <string>
using namespace std;
class Animal05{
public:
    Animal05(){
        cout<<"Animal构造调用"<<endl;
    }
    //虚函数
    virtual void speak(){
        cout<<"animal speak"<<endl;
    }
//    ~Animal05(){
//        cout<<"Animal析构调用"<<endl;
//    }
    //虚析构-可以解决父类指针释放子类对象不干净的问题
//    virtual  ~Animal05(){
//        cout<<"Animal析构调用"<<endl;
//    }
    //纯虚析构-需要声明也需要实现
    // 有了纯虚析构之后，该类属于抽象类无法实例化对象
    virtual ~Animal05() = 0;
};
Animal05::~Animal05() {
    cout<<"Animal纯虚析构调用"<<endl;
}
class Cat05:public Animal05{
public:
    string *name;
    Cat05(string n){
        cout<<"Cat构造调用"<<endl;
        name = new string(n);
    }
    void speak(){
        cout<<*name<<" cat speak"<<endl;
    }
    ~Cat05(){
        if(name != NULL){
            cout<<"Cat析构调用"<<endl;
            delete name;
            name = NULL;
        }
    }
};
class Dog05:public Animal05{
public:
    void speak(){
        cout<<"Dog speak"<<endl;
    }
};
void  test0501(){
    Animal05 *animal = new Cat05("Tom");
    animal->speak();
    //父类指针在析构时候 不会调用子类中析构函数，导致子类如果有堆区属性会出现内存泄漏
    delete animal;
}
//虚析构和纯虚析构
int main(){
    test0501();
    return 0;
}
```
![](https://i.imgur.com/z8MxLTx.png)

#### 4.7.6多态案例-电脑组装

![](https://i.imgur.com/ZPP3t0X.png)

```c++
//
// Created by JSQ on 2021/7/6.
//
#include <iostream>
#include <string>
using namespace std;
class CPU
{
public:
    virtual void calculate() = 0;
};
class GPU
{
public:
    virtual void display() = 0;
};
class Memory
{
public:
    virtual void storage() = 0;
};
class Computer{
public:
    Computer(CPU *_cpu,GPU *_gpu,Memory *_mem){
        cpu = _cpu;
        gpu = _gpu;
        mem = _mem;
    }
    void work(){
        cpu->calculate();
        gpu->display();
        mem->storage();
    }
    virtual ~Computer(){
        if(cpu != NULL){
            delete cpu;
            cpu = NULL;
        }
        if(gpu != NULL){
            delete gpu;
            gpu = NULL;
        }
        if(mem != NULL){
            delete mem;
            mem = NULL;
        }
    }
private:
    CPU *cpu;
    GPU *gpu;
    Memory *mem;
};
//Intel
class IntelCPU:public CPU{
public:
    virtual void calculate(){
        cout<<"cpu of Intel is calculating!"<<endl;
    }
};
class IntelGPU:public GPU{
public:
    virtual void display(){
        cout<<"gpu of Intel is displaying!"<<endl;
    }
};
class IntelMem:public Memory{
public:
    virtual void storage(){
        cout<<"Memory of Intel is storing!"<<endl;
    }
};
//Lenovo
class LenovoCPU:public CPU{
public:
    virtual void calculate(){
        cout<<"cpu of Lenovo is calculating!"<<endl;
    }
};
class LenovoGPU:public GPU{
public:
    virtual void display(){
        cout<<"gpu of Lenovo is displaying!"<<endl;
    }
};
class LenovoMem:public Memory{
public:
    virtual void storage(){
        cout<<"Memory of Lenovo is storing!"<<endl;
    }
};
void test0601(){
    //1
    CPU *intelCpu = new IntelCPU;
    GPU *intelGpu = new IntelGPU;
    Memory *intelMem = new IntelMem;
    Computer *computer1 = new Computer(intelCpu,intelGpu,intelMem);
    computer1->work();
    delete computer1;

    //2
    Computer *computer2 = new Computer(new LenovoCPU, new IntelGPU,new LenovoMem);
    computer2->work();
    delete computer2;
}
int main(){
    test0601();
    return 0;
}
```

## 5. 文件操作

![](https://i.imgur.com/pvOdGbc.png)

### 5.1 文本文件

#### 5.1.1写文件

![](https://i.imgur.com/jq6tlXt.png)

![](https://i.imgur.com/qssyLyM.png)

![](https://i.imgur.com/KlvCL7j.png)

```c++
//
// Created by JSQ on 2021/7/5.
//文本文件 写文件
#include <iostream>
#include <string>
#include <fstream>
using namespace std;
void test0101(){
    //1.包含头文件
    //2.创建流对象
    ofstream  ofs;
    //3.指定打开方式
    ofs.open("text.txt",ios::out);
    //4.写内容
    ofs<<"my name is zhangsan "<<endl;
    ofs<<"my name is zhangsan "<<endl;
    ofs<<"my name is zhangsan "<<endl;
    //5.关闭文件
    ofs.close();
}
int main(){
    test0101();
    return 0;
}
```
![](https://i.imgur.com/AqF1Axg.png)

#### 5.1.2读文件

![](D:\JYH\Jobs\C++_learning\CppKernel\Mynotes.assets\PwM5IhI.png)

```c++
//
// Created by JSQ on 2021/7/5.
//文本文件 读文件
#include <iostream>
#include <string>
#include <fstream>
using namespace std;
void test0201(){
    //1.包含头文件
    //2.创建流对象
    ifstream  ifs;
    //3.打开文件 并且判断是否打开成功
    ifs.open("text.txt",ios::in);
    if(!ifs.is_open()){
        cout<<"File open failed!"<<endl;
        return;
    }
    //4.读数据
    //第一种
//    char buf[1024]= {0};
//    while (ifs>>buf){
//        cout<<buf<<endl;
//    }
    //第二种
//    char buf[1024]= {0};
//    while (ifs.getline(buf,sizeof(buf))){
//        cout<<buf<<endl;
//    }
    //第三种
//    string buf;
//    while (getline(ifs, buf)){
//        cout<<buf<<endl;
//    }
    //第四种
    char buf;
    while ((buf = ifs.get())!= EOF){//EOF end of file
        cout<<buf;
    }
    //5.关闭文件
    ifs.close();
}
int main(){
    test0201();
    return 0;
}
```
![](https://i.imgur.com/2UtnZyc.png)

### 5.2二进制文件

![](https://i.imgur.com/UKF0uSW.png)

#### 5.2.1写文件

![](https://i.imgur.com/4MgreQ5.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
#include <fstream>
using namespace std;
//二进制文件 写操作
class Person01{
public:
    char name[64];
    int age;
};
void test01(){
//    fstream ofs;
//    ofs.open("Person.txt",ios::binary|ios::out);
    fstream ofs("Person.txt",ios::binary|ios::out);//二合一
    Person01 p = {"张三",18};
    ofs.write((const char *)&p,sizeof(Person01));
    ofs.close();
}
int main(){
    test01();
    return 0;
}
```
![](D:\JYH\Jobs\C++_learning\CppKernel\Mynotes.assets\dDwS25P.png)

#### 5.2.2读文件

![](https://i.imgur.com/PfXD8Up.png)

```c++
//
// Created by JSQ on 2021/7/5.
//
#include <iostream>
#include <string>
#include <fstream>
using namespace std;
//二进制文件 写操作
class Person01{
public:
    char name[64];
    int age;
};
void test01(){
//    fstream ofs;
//    ofs.open("Person.txt",ios::binary|ios::out);
    fstream ofs("Person.txt",ios::binary|ios::out);//二合一
    Person01 p = {"Lihua",18};
    ofs.write((const char *)&p,sizeof(Person01));
    ofs.close();
}
int main01(){
    test01();
    return 0;
}
```

