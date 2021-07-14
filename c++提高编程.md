# c++提高编程

```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
    
    return 0;
}
```

- C++泛型编程和STL技术

## 1.模板

### 1.1 模板的概念

![](https://i.imgur.com/0LSfF5W.png)

### 1.2 函数模板

![](https://i.imgur.com/lwmRrEq.png)

#### 1.2.1 函数模板语法

![](https://i.imgur.com/WUguZR4.png)

```c++
#include <iostream>
#include <string>
using namespace std;
//两数交换函数
template <typename T>
void swapT(T &a, T &b){
    T temp = a;
    a = b;
    b = temp;
}
void test01(){
    int a = 10;
    int b = 20;
    char c= 's';
    cout<<a<<","<<b<<endl;
    //自动类型推导
    swapT(a,b);
    //显式指定类型
    swapT<int>(a,b);
    cout<<a<<","<<b<<endl;

}
int main(){
    test01();
    return 0;
}
```

![](https://i.imgur.com/a0muotI.png)

#### 1.2.2 函数模板注意事项
```c++
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//函数模板的注意事项
// 1.自动类型推导，必须要推导出一致的类型才可使用
//2. 模板必须要确定出T的数据类型才可使用

template <typename T>
void swapT(T &a, T &b){
    T temp = a;
    a = b;
    b = temp;
}
template <typename T>
void func(){
    cout<<"func"<<endl;
}
void test01(){
    int a = 10;
    int b = 20;
    char c= 's';
    cout<<a<<","<<b<<endl;
    //自动类型推导
    swapT(a,b);
//    swapT(a,c);//报错-推导类型不一致
    //显式指定类型
//    swapT<int>(a,b);
    cout<<a<<","<<b<<endl;

}
int main(){
    test01();
//    func();//当前函数无法确定T的数据类型
    func<int>();
    return 0;
}
```
![](https://i.imgur.com/youmj4Y.png)

#### 1.2.3 函数模板案例

![](D:\JYH\mydaily\PublicNoteBooks\c++提高编程.assets\ZkmhPoY.png)

```c++
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//通用数组排序
// char int

template <typename T>
void swapT(T &a, T &b){
    T temp = a;
    a = b;
    b = temp;
}
template <typename T>
void mySort(T arr[], int len){
    for(int i =0;i<len;i++){
        int max = i;
        for(int j = i +1;j<len;j++){
            if(arr[max]<arr[j]){
                max = j;
            }
        }
        if(max != i){
            swapT(arr[i],arr[max]);
        }
    }
}
template<typename T>
void printArr(T arr[], int len){
    for (int i = 0;i<len;i++){
        cout<<arr[i]<<" ";
    }
    cout<<endl;
}
void test01(){
    char charArr[] = "absdklf";
    int charlen = sizeof(charArr)/ sizeof(char);
    mySort(charArr,charlen);
    printArr(charArr, charlen);
}
void test02(){
    int intArr[] = {3,4,5,6,3,5,7,8,3};
    int intlen = sizeof(intArr)/ sizeof(int);
    mySort(intArr,intlen);
    printArr(intArr,intlen);
}
int main(){
    test01();
    test02();
    return 0;
}
```
#### 1.2.4 普通函数与函数模板的区别

![](https://i.imgur.com/uWOgeXD.png)

```c++
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//普通函数与函数模板的区别
// 1.普通函数调用可以发生隐式类型转换
// 2.函数模板 用自动类型推导，不可以发生隐式类型转换
// 3.函数模板 用显式类型推导，可以发生隐式类型转换
int myAdd(int a, int b){
    return a + b;
}
template<typename T>
int myAddT(T a, T b){
    return a + b;
}
void test01(){
    int a = 10;
    int b = 20;
    char c = 'c';//a- 97, c-99转成ascii码
    cout<<myAdd(a,c)<<endl;
//    cout<<myAddT(a,c)<<endl;//报错
    cout<<myAddT<int>(a,c)<<endl;
}

int main(){
    test01();
    return 0;
}
```
#### 1.2.5 普通函数与函数模板的调用规则

![](https://i.imgur.com/wkWrmbv.png)

```c++
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//普通函数与函数模板的调用规则
// 1.函数模板和普通函数都可以调用，优先普通函数
// 2.可以通过空模板参数列表强制调用函数模板
// 3.函数模板可以发生函数重载
// 4.函数模板如果可以产生更好匹配，优先调用函数模板
void myPrint(int a, int b)
{
    cout<<"Simple"<<endl;
}
template<typename T>
void myPrint(T a, T b){
    cout<<"Template"<<endl;
}
//重载
template<typename T>
void myPrint(T a, T b, T c){
        cout<<"Template overload"<<endl;
}
void test01(){
    int a = 10;
    int b = 20;
    myPrint(a,b);
    //空模板-强制调用
    myPrint<>(a,b);
    //重载
    myPrint(a,b,100);
    //优先调用
    char c1 = 'a';
    char c2 = 'b';
    myPrint(c1,c2);

}

int main(){
    test01();
    return 0;
}
```
![](D:\JYH\mydaily\PublicNoteBooks\c++提高编程.assets\ZiZO7DJ.png)

#### 1.2.6 模板的局限性

**局限性：**

- 模板的通用性并不是万能的

**例如：**

```cpp
template<class T>
void f(T a, T b)
{ 
    a = b;
}

```

在上述代码中提供的赋值操作，如果传入的a和b是一个数组，就无法实现了

再例如：

```cpp
template<class T>
void f(T a, T b)
{ 
    if(a > b) { ... }
}

```

在上述代码中，如果T的数据类型传入的是像Person这样的自定义数据类型，也无法正常运行

因此C++为了解决这种问题，提供模板的重载，可以为这些**特定的类型**提供**具体化的模板**

```c++
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//模板的局限性
class Person{
public:
    Person(string n, int a){
        this->name = n;
        this->age = a;
    }
    string name;
    int age;
};

template<typename T>
bool myCMP(T &a, T &b){
    if(a==b){
        return true;
    } else{
        return false;
    }
}
//利用具体化Person版本实现代码，具体化优先调用
template<> bool myCMP(Person &a, Person &b)
{
    if(a.age == b.age && a.name == b.name){
        return true;
    } else{
        return false;
    }
}
//重载

void test01(){
    int a = 10;
    int b = 20;
    string ret = myCMP(a,b)?"==":"!=";
    cout<<ret<<endl;
}
void test02(){
    Person p1("Tom",10);
    Person p2("Tom",10);
    string ret = myCMP(p1,p2)?"==":"!=";
    cout<<ret<<endl;
}
int main(){
    test02();
    return 0;
}

```
总结：

- 利用具体化的模板，可以解决自定义类型的通用化
- 学习模板并不是为了写模板，而是在STL能够运用系统提供的模板

### 1.3 类模板

#### 1.3.1 类模板语法

类模板作用：

- 建立一个通用类，类中的成员 数据类型可以不具体制定，用一个**虚拟的类型**来代表。

**语法：**

```cpp
template<typename T>
类
```

**解释：**

template — 声明创建模板

typename — 表面其后面的符号是一种数据类型，可以用class代替

T — 通用的数据类型，名称可以替换，通常为大写字母

**示例：**

```cpp
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//类模板
template<class NameType,class AgeType>
class Person{
public:
    Person(NameType n, AgeType a){
        this->name = n;
        this->age = a;
    }
    void ShowPerson(){
        cout<<this->name<<","<< this->age<<endl;
    }
    string name;
    int age;
};

void test01(){
    Person<string, int>p1("Tom",18);
    p1.ShowPerson();
}

int main(){
    test01();
    return 0;
}
```

总结：类模板和函数模板语法相似，在声明模板template后面加类，此类称为类模板

#### 1.3.2 类模板与函数模板区别

类模板与函数模板区别主要有两点：

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数

**示例：**

```cpp
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//类模板
template<class NameType,class AgeType = int>
class Person{
public:
    Person(NameType n, AgeType a){
        this->name = n;
        this->age = a;
    }
    void ShowPerson(){
        cout<<this->name<<","<< this->age<<endl;
    }
    string name;
    int age;
};
//1.类模板没有自动类型推导使用方式
void test01(){
//    Person<>p1("Tom",18);//报错，无法用自动类型推导
    Person<string, int>p1("Tom",18);
    p1.ShowPerson();
}
//2.类模板在模板参数列表中可以有默认参数
void test02(){
    Person<string>p1("Jerry",20);//有默认参数
    p1.ShowPerson();
}
int main(){
    test02();
    return 0;
}
```

总结：

- 类模板使用只能用显示指定类型方式
- 类模板中的模板参数列表可以有默认参数

#### 1.3.3 类模板中成员函数创建时机

类模板中成员函数和普通类中成员函数创建时机是有区别的：

- 普通类中的成员函数一开始就可以创建
- 类模板中的成员函数在调用时才创建

**示例：**

```cpp
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//类模板中成员函数的创建时机
// 类模板中成员函数在调用时才去创建
class Person1{
public:
    void ShowPerson1(){
        cout<<"Person1"<<endl;
    }
};
class Person2{
public:
    void ShowPerson2(){
        cout<<"Person2"<<endl;
    }
};
template<class T>
class MyClass{
public:
    T obj;
    //类模板中的成员函数
    void func1(){
        obj.ShowPerson1();
    }
    void func2(){
        obj.ShowPerson2();
    }
};
void test01(){
    MyClass<Person1>m;
    m.func1();
//    m.func2();
}

int main(){
    test01();
    return 0;
}
```

总结：类模板中的成员函数并不是一开始就创建的，在调用时才去创建

#### 1.3.4 类模板对象做函数参数

学习目标：

- 类模板实例化出的对象，向函数传参的方式

一共有三种传入方式：

1. 指定传入的类型 — 直接显示对象的数据类型
2. 参数模板化 — 将对象中的参数变为模板进行传递
3. 整个类模板化 — 将这个对象类型 模板化进行传递

**示例：**

```cpp
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//类模板的对象做函数参数
template<class T1,class T2>
class Person{
public:
    Person(T1 n, T2 a){
        this->name = n;
        this->age = a;
    }
    void showperson(){
        cout<< this->name<<","<< this->age<<endl;
    }
    T1 name;
    T2 age;
};
// 1.指定传入类型
void printPerson(Person<string,int>&p){
    p.showperson();
}
void test01(){
    Person<string,int>p1("zhangsan",18);
    printPerson(p1);
}
// 2.参数模板化
template<class T1,class T2>
void printPerson2(Person<T1,T2>&p){
    p.showperson();
    cout<< typeid(T1).name()<<endl;
    cout<< typeid(T2).name()<<endl;

}
void test02(){
    Person<string,int>p1("Lihua",20);
    printPerson2(p1);
}
// 3.整个类模板化
template<class T>
void printPerson3(T &p){
    p.showperson();
    cout<< typeid(T).name()<<endl;
//    cout<< typeid(T2).name()<<endl;

}
void test03(){
    Person<string,int>p1("Wangwu",25);
    printPerson3(p1);
}


int main(){
    test01();
    test02();
    test03();
    return 0;
}
```

总结：

- 通过类模板创建的对象，可以有三种方式向函数中进行传参
- 使用比较广泛是第一种：指定传入的类型

#### 1.3.5 类模板与继承

当类模板碰到继承时，需要注意一下几点：

- 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
- 如果不指定，编译器无法给子类分配内存
- 如果想灵活指定出父类中T的类型，子类也需变为类模板

**示例：**

```cpp
//
// Created by JSQ on 2021/7/7.
//
#include <iostream>
#include <string>
using namespace std;
//类模板与继承
template<class T>
class Base{
    T m;
};
class Son:public Base<int>//必须要知道父类中的T类型才能继承
{

};


//如果想要里灵活指定父类中T类型，子类也需要变成类模板
template<class T1,class T2>
class Son2:public Base<T2>{
public:
    Son2(){
        cout<< typeid(T1).name()<<endl;
        cout<< typeid(T2).name()<<endl;
    }
    T1 obj;
};

void test01(){
    Son2<int,char>S2;
}
int main(){
    test01();
    return 0;
}
```

总结：如果父类是类模板，子类需要指定出父类中T的数据类型

#### 1.3.6 类模板成员函数类外实现

学习目标：能够掌握类模板中的成员函数类外实现

**示例：**

```cpp
//
// Created by JSQ on 2021/7/8.
//
#include <iostream>
#include <string>
using namespace std;
//类模板中成员函数的类外实现
template<class T1, class T2>
class Person{
public:
    Person(T1 n,T2 a);
//    {
//        this->name = n;
//        this->age = a;
//    }
    void ShowPerson();
//    {
//        cout<<this->name<<","<< this->age<<endl;
//    }
    string name;
    int age;
};
//构造函数的类外实现
template<class T1, class T2>
Person<T1,T2>::Person(T1 n,T2 a){
    this->name = n;
    this->age = a;
}
//成员函数的类外实现
template<class T1, class T2>
void Person<T1,T2>::ShowPerson(){
    cout<<this->name<<","<< this->age<<endl;
}
void test01(){
    Person<string,int>p1("Tom",20);
    p1.ShowPerson();
}
int main(){
    test01();
    return 0;
}
```

总结：类模板中成员函数类外实现时，需要加上模板参数列表

#### 1.3.7 类模板分文件编写

学习目标：

- 掌握类模板成员函数分文件编写产生的问题以及解决方式

问题：

- 类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到

解决：

- 解决方式1：直接包含.cpp源文件
- 解决方式2：将声明和实现写到同一个文件中，并更改后缀名为.hpp，hpp是约定的名称，并不是强制

**示例：**

person.hpp中代码：

```cpp
//
// Created by JSQ on 2021/7/8.
//

#ifndef INC_1_3_CLASS_TEMPLATE_PERSON_HPP
#define INC_1_3_CLASS_TEMPLATE_PERSON_HPP

#endif //INC_1_3_CLASS_TEMPLATE_PERSON_HPP
#pragma once
#include <iostream>
using namespace std;
#include <string>
//类模板分文件编写问题以及解决
template<class T1, class T2>
class Person{
public:
    Person(T1 n,T2 a);
    void ShowPerson();
    string name;
    int age;
};
//构造函数的类外实现
template<class T1, class T2>
Person<T1,T2>::Person(T1 n,T2 a){
    this->name = n;
    this->age = a;
}
//成员函数的类外实现
template<class T1, class T2>
void Person<T1,T2>::ShowPerson(){
    cout<<this->name<<","<< this->age<<endl;
}
```

类模板分文件编写.cpp中代码

```cpp
//
// Created by JSQ on 2021/7/8.
//
#include <iostream>
#include <string>
using namespace std;
//#include "person.cpp"
//若使用 person.h文件，会由于创建时机的问题，
// 编译器一开始不会创建类中的成员函数等内容，因而后续程序运行过程会报错
//若使用 person.cpp文件，就可以正常的产生相关内容，但由于实际过程中一般不会直接暴露源码
//将.h和.cpp写在一起，改成.hpp文件
#include "person.hpp"
//类模板分文件编写问题以及解决

void test01(){
    Person<string,int>p1("Tom",18);
    p1.ShowPerson();
}
int main(){
    test01();
    return 0;
}
```

总结：主流的解决方式是第二种，将类模板成员函数写到一起，并将后缀名改为.hpp

#### 1.3.8 类模板与友元

学习目标：

- 掌握类模板配合友元函数的类内和类外实现

全局函数类内实现 - 直接在类内声明友元即可

全局函数类外实现 - 需要提前让编译器知道全局函数的存在

**示例：**

```cpp
//
// Created by JSQ on 2021/7/8.
//
#include <iostream>
#include <string>
using namespace std;
//通过全局函数 打印信息
//提前让编译器知道类存在
template<class T1, class T2>
class Person;
//类外实现
template<class T1, class T2>
void printPerson1(Person<T1,T2> p){
    cout<<p.name<<","<< p.age<<endl;
}
template<class T1, class T2>
class Person{
    //全局函数类内实现
    friend void printPerson(Person<T1,T2> p){
        cout<<p.name<<","<< p.age<<endl;
    }
    //全局函数类外实现
    //加空模板参数列表
    //如果全局函数是类外实现，需要让编译器提前知道该函数的存在
    friend void printPerson1<>(Person<T1,T2> p);
public:
    Person(T1 n,T2 a)
    {
        this->name = n;
        this->age = a;
    }
    void ShowPerson()
    {
        cout<<this->name<<","<< this->age<<endl;
    }
    string name;
    int age;
};
void test01(){
    Person<string,int>p1("Tom",18);
    printPerson(p1);
    printPerson1(p1);
}
int main(){
    test01();
    return 0;
}
```

总结：建议全局函数做类内实现，用法简单，而且编译器可以直接识别

#### 1.3.9 类模板案例

案例描述: 实现一个通用的数组类，要求如下：

- 可以对内置数据类型以及自定义数据类型的数据进行存储
- 将数组中的数据存储到堆区
- 构造函数中可以传入数组的容量
- 提供对应的拷贝构造函数以及operator=防止浅拷贝问题
- 提供尾插法和尾删法对数组中的数据进行增加和删除
- 可以通过下标的方式访问数组中的元素
- 可以获取数组中当前元素个数和数组的容量

**示例：**

myArray.hpp中代码

```cpp
//
// Created by JSQ on 2021/7/8.
//

#ifndef INC_1_3_CLASS_TEMPLATE_MYARRAY_HPP
#define INC_1_3_CLASS_TEMPLATE_MYARRAY_HPP

#endif //INC_1_3_CLASS_TEMPLATE_MYARRAY_HPP
#pragma once
#include <iostream>
#include <string>
using namespace std;
//通用数组类
template<class T>
class MyArray{
private:
    T *address;//指针指向堆区开辟的真实数组
    int capacity;
    int size;
public:
    //有参构造
    MyArray(int cap){
        cout<<"有参构造调用"<<endl;
        this->capacity = cap;
        this->size = 0;
        this->address = new T[this->capacity];
    }
    //拷贝构造
    MyArray(const MyArray &arr){
        cout<<"拷贝构造调用"<<endl;
        this->capacity = arr.capacity;
        this->size = arr.size;
//        this->address = arr.address;//浅拷贝
        //深拷贝
        this->address = new T[arr.capacity];
        //将arr中的数据都拷贝过来
        for(int i =0 ;i< this->size;i++){
            this->address[i] = arr.address[i];
        }
    }
    //operator= 防止浅拷贝的问题
    MyArray& operator=(const MyArray &arr){
        cout<<"等号重载调用"<<endl;
        //先判断原来堆区是否有数据，如有先释放
        if(this->address != NULL){
            delete [] this->address;
            this->address = NULL;
            this->capacity = 0;
            this->size = 0;
        }
        //深拷贝
        this->capacity = arr.capacity;
        this->size = arr.size;
        this->address = new T[arr.capacity];
        //将arr中的数据都拷贝过来
        for(int i =0 ;i< this->size;i++){
            this->address[i] = arr.address[i];
        }
        return *this;
    }
    //尾插法
    void Push_Back(const T &val){
        //判断容量
        if(this->capacity == this->size){
            cout<<"当前容量已满，无法插入"<<endl;
            return;
        }
        this->address[this->size] = val;
        this->size ++;
    }
    //尾删法
    void Pop_Back(){
        //让用户访问不到最后一个元素，逻辑删除
        if(this->size == 0){
            cout<<"当前没有任何元素"<<endl;
            return;
        }
        this->size--;
    }
    //让用户以下标方式访问数组的元素
    T& operator[](int index){
        return this->address[index];
    }
    //返回数组容量
    int getCapacity(){
        return this->capacity;
    }
    //返回数组大小
    int getSize(){
        return this->size;
    }
    //析构函数
    ~MyArray(){
        cout<<"析构调用"<<endl;
        if(this->address != NULL){
            delete [] this->address;
            this->address = NULL;
        }
    }
};
```

类模板案例—数组类封装.cpp中

```cpp
//
// Created by JSQ on 2021/7/8.
//
#include <iostream>
#include <string>
#include "myarray.hpp"
using namespace std;
void printIntArray(MyArray<int> &a1){
    cout<<"打印输出"<<endl;
    for (int i = 0; i < a1.getSize(); ++i) {
        cout<<a1[i]<<" ";
    }
    cout<<endl;
}
void test01(){
    MyArray<int>a1(5);
    for (int i = 0; i < 5; i++) {
        a1.Push_Back(i);
    }
    printIntArray(a1);
    cout<<"capacity:"<<a1.getCapacity()<<endl;
    cout<<"size:"<<a1.getSize()<<endl;
    MyArray<int>a2(a1);
    a2.Pop_Back();
    cout<<"capacity:"<<a2.getCapacity()<<endl;
    cout<<"size:"<<a2.getSize()<<endl;
    printIntArray(a2);
}
//测试自定义数据类型
class Person{
public:
    Person(){};
    Person(string name, int age){
        this->m_Age = age;
        this->m_Name = name;
    };
    string m_Name;
    int m_Age;
};
void printPersonArray(MyArray<Person> &a1){
    cout<<"打印输出"<<endl;
    for (int i = 0; i < a1.getSize(); ++i) {
        cout<<a1[i].m_Name<<": "<<a1[i].m_Age<<endl;
    }
}

void test02(){
    MyArray<Person>arr(10);
    Person p1("Tom",22);
    Person p2("Jerry",19);
    Person p3("David",25);
    Person p4("Grace",15);
    Person p5("Jenny",17);
    //将数据插入到数组中
    arr.Push_Back(p1);
    arr.Push_Back(p2);
    arr.Push_Back(p3);
    arr.Push_Back(p4);
    arr.Push_Back(p5);
    printPersonArray(arr);
    cout<<"capacity:"<<arr.getCapacity()<<endl;
    cout<<"size:"<<arr.getSize()<<endl;
}
int main(){
//    test01();
    test02();
    return 0;
}
```

总结：

能够利用所学知识点实现通用的数组

## 2 STL初识

### 2.1 STL的诞生

- 长久以来，软件界一直希望建立一种可重复利用的东西

- C++的**面向对象**和**泛型编程**思想，目的就是**复用性的提升**

- 大多情况下，数据结构和算法都未能有一套标准,导致被迫从事大量重复工作

- 为了建立数据结构和算法的一套标准,诞生了**STL**

  

### 2.2 STL基本概念

- STL(Standard Template Library,**标准模板库**)
- STL 从广义上分为: **容器(container) 算法(algorithm) 迭代器(iterator)**
- **容器**和**算法**之间通过**迭代器**进行无缝连接。
- STL 几乎所有的代码都采用了模板类或者模板函数

### 2.3 STL六大组件

STL大体分为六大组件，分别是:**容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器**

1. 容器：各种数据结构，如vector、list、deque、set、map等,用来存放数据。
2. 算法：各种常用的算法，如sort、find、copy、for_each等
3. 迭代器：扮演了容器与算法之间的**胶合剂**。
4. 仿函数：行为类似函数，可作为算法的某种策略。
5. 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西。
6. 空间配置器：负责空间的配置与管理。

### 2.4 STL中容器、算法、迭代器

容器：**置物之所也**

STL**容器**就是将运用**最广泛的一些数据结构**实现出来

常用的数据结构：数组, 链表,树, 栈, 队列, 集合, 映射表 等

这些容器分为**序列式容器**和**关联式容器**两种:

 **序列式容器**:强调值的排序，序列式容器中的每个元素均有固定的位置。
**关联式容器**:二叉树结构，各元素之间没有严格的物理上的顺序关系



**算法**：问题之解法也

有限的步骤，解决逻辑或数学上的问题，这一门学科我们叫做算法(Algorithms)

算法分为:**质变算法**和**非质变算法**。

质变算法：是指运算过程中会更改区间内的元素的内容。例如拷贝，替换，删除等等

非质变算法：是指运算过程中不会更改区间内的元素内容，例如查找、计数、遍历、寻找极值等等



**迭代器**：容器和算法之间粘合剂-算法要通过迭代器才能访问容器中的元素

提供一种方法，使之能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部表示方式。

每个容器都有自己专属的迭代器

迭代器使用非常类似于指针，初学阶段我们可以先理解迭代器为指针



迭代器种类：

| 种类           | 功能                                                     | 支持运算                               |
| -------------- | -------------------------------------------------------- | -------------------------------------- |
| 输入迭代器     | 对数据的只读访问                                         | 只读，支持++、==、！=                  |
| 输出迭代器     | 对数据的只写访问                                         | 只写，支持++                           |
| 前向迭代器     | 读写操作，并能向前推进迭代器                             | 读写，支持++、==、！=                  |
| 双向迭代器     | 读写操作，并能向前和向后操作                             | 读写，支持++、–，                      |
| 随机访问迭代器 | 读写操作，可以以跳跃的方式访问任意数据，功能最强的迭代器 | 读写，支持++、–、[n]、-n、<、<=、>、>= |

常用的容器中迭代器种类为双向迭代器，和随机访问迭代器



### 2.5 容器算法迭代器初识

了解STL中容器、算法、迭代器概念之后，我们利用代码感受STL的魅力

STL中最常用的容器为Vector，可以理解为数组，下面我们将学习如何向这个容器中插入数据、并遍历这个容器

#### 2.5.1 vector存放内置数据类型

容器： `vector`

算法： `for_each`

迭代器： `vector<int>::iterator`

**示例：**

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
void myprint(int val){
    cout<<val<<endl;
};
//vector 容器存放内置数据类型
void test01(){
//    创建了一个vector容器，数组
    vector<int> v;
//    插入shuju
    v.push_back(10);
    v.push_back(20);
    v.push_back(30);
    v.push_back(40);
    v.push_back(50);
////    通过迭代器访问容器中的数据
//    vector<int>::iterator itBegin = v.begin();//起始迭代器，指向第一个元素
//    vector<int>::iterator itEnd = v.end();//结束迭代器，指向最后一个元素的下一个位置
////    第一种遍历方式
//    while(itBegin!=itEnd){
//        cout<<*itBegin<<endl;
//        itBegin++;
//    }
//    第2种遍历方式
//    for(vector<int>::iterator it = v.begin();it!=v.end();it++){
//        cout<<*it<<endl;
//    }
//    第3种遍历方式
//利用STL提供的遍历算法
    for_each(v.begin(),v.end(),myprint);//利用回调函数
}
int main(){
    test01();
    return 0;
}
```

#### 2.5.2 Vector存放自定义数据类型

学习目标：vector中存放自定义数据类型，并打印输出

**示例：**

```cpp
//
// Created by JSQ on 2021/7/8.
//
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

//vector 容器存放自定义数据类型
class Person{
public:
    Person(string n, int a){
        this->age = a;
        this->name = n;
    }
    string name;
    int age;

};
void test01(){
    vector<Person> v;
    Person p1("Tom",22);
    Person p2("Jerry",19);
    Person p3("David",25);
    Person p4("Grace",15);
    Person p5("Jenny",17);
    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    v.push_back(p4);
    v.push_back(p5);
    for(vector<Person>::iterator it = v.begin();it != v.end(); it++){
//        cout<<(*it).name<<": "<<(*it).age<<endl;
        cout<<it->name<<": "<<it->age<<endl;
    }
}
//存放自定义数据类型指针
void test02(){
    vector<Person*> v;
    Person p1("Tom",22);
    Person p2("Jerry",19);
    Person p3("David",25);
    Person p4("Grace",15);
    Person p5("Jenny",17);
    v.push_back(&p1);
    v.push_back(&p2);
    v.push_back(&p3);
    v.push_back(&p4);
    v.push_back(&p5);
    for(vector<Person*>::iterator it = v.begin();it != v.end(); it++){
        cout<<(*it)->name<<": ~~~"<<(*it)->age<<endl;
//        cout<<it->name<<": "<<it->age<<endl;
    }
}

int main(){
    test02();
    return 0;
}
```

#### 2.5.3 Vector容器嵌套容器

学习目标：容器中嵌套容器，我们将所有数据进行遍历输出

**示例：**

```cpp
//
// Created by JSQ on 2021/7/8.
//
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

//vector 容器嵌套容器

void test01(){
    vector<vector<int>> v;
    vector<int> v1;
    vector<int> v2;
    vector<int> v3;
    vector<int> v4;
    vector<int> v5;
    //向小容器添加数据
    for (int i = 0; i < 5; ++i) {
        v1.push_back(i+1);
        v2.push_back(i+2);
        v3.push_back(i+3);
        v4.push_back(i+4);
        v5.push_back(i+5);
    }
    //将小容器插入大容器
    v.push_back(v1);
    v.push_back(v2);
    v.push_back(v3);
    v.push_back(v4);
    v.push_back(v5);
    //通过大容器遍历所有数据
    for(vector<vector<int>>::iterator it = v.begin(); it != v.end();it++){
        for(vector<int>::iterator vit = (*it).begin(); vit != (*it).end();vit++){
            cout<<*vit<<" ";
        }
        cout<<endl;
    }
}


int main(){
    test01();
    return 0;
}
```

## 3.STL常用容器

### 3.1 string容器

#### 3.1.1 string基本概念

**本质：**

- string是C++风格的字符串，而string本质上是一个类

**string和char \* 区别：**

- char * 是一个指针
- string是一个类，类内部封装了char*，管理这个字符串，是一个char*型的容器。

**特点：**

string 类内部封装了很多成员方法

例如：查找find，拷贝copy，删除delete 替换replace，插入insert

string管理char*所分配的内存，不用担心复制越界和取值越界等，由类内部进行负责

#### 3.1.2 string构造函数

构造函数原型：

- `string();` //创建一个空的字符串 例如: string str;
  `string(const char* s);` //使用字符串s初始化
- `string(const string& str);` //使用一个string对象初始化另一个string对象
- `string(int n, char c);` //使用n个字符c初始化

**示例：**

```cpp
#include <iostream>
#include <string>
using namespace std;
//string的构造函数
/*
- `string();` //创建一个空的字符串 例如: string str;
  `string(const char* s);` //使用字符串s初始化
- `string(const string& str);` //使用一个string对象初始化另一个string对象
- `string(int n, char c);` //使用n个字符c初始化
 */
void test01(){
    string s1;
    const char *str = "hello world";
    string s2(str);
    cout<<s2<<endl;
    string s3(s2);
    string s4(10,'a');
    cout<<s3<<endl;
    cout<<s4<<endl;
}
int main(){
    test01();
    return 0;
}
```

总结：string的多种构造方式没有可比性，灵活使用即可

#### 3.1.3 string赋值操作

功能描述：

- 给string字符串进行赋值

赋值的函数原型：

- `string& operator=(const char* s);` //char*类型字符串 赋值给当前的字符串
- `string& operator=(const string &s);` //把字符串s赋给当前的字符串
- `string& operator=(char c);` //字符赋值给当前的字符串
- `string& assign(const char *s);` //把字符串s赋给当前的字符串
- `string& assign(const char *s, int n);` //把字符串s的前n个字符赋给当前的字符串
- `string& assign(const string &s);` //把字符串s赋给当前字符串
- `string& assign(int n, char c);` //用n个字符c赋给当前字符串

**示例：**

```cpp
#include <iostream>
#include <string>
using namespace std;
//string的构造函数
/*
- `string& operator=(const char* s);` //char*类型字符串 赋值给当前的字符串
- `string& operator=(const string &s);` //把字符串s赋给当前的字符串
- `string& operator=(char c);` //字符赋值给当前的字符串
- `string& assign(const char *s);` //把字符串s赋给当前的字符串
- `string& assign(const char *s, int n);` //把字符串s的前n个字符赋给当前的字符串
- `string& assign(const string &s);` //把字符串s赋给当前字符串
- `string& assign(int n, char c);` //用n个字符c赋给当前字符串
 */
void test01(){
    string s1;
    s1 = "abc";
    cout<<s1<<endl;
    string s2 ;
    s2 = s1;
    cout<<s2<<endl;
    string s3 ;
    s3 = 'a';
    cout<<s3<<endl;
    string s4 ;
    s4.assign("hello");
    cout<<s4<<endl;
    string s5 ;
    s5.assign("hello",3);
    cout<<s5<<endl;
    string s6;
    s6.assign(10,'a');
    cout<<s6<<endl;
    string s7;
    s7.assign(s5);
    cout<<s7<<endl;

}
int main(){
    test01();
    return 0;
}
```

总结：

 string的赋值方式很多，`operator=` 这种方式是比较实用的

#### 3.1.4 string字符串拼接

**功能描述：**

- 实现在字符串末尾拼接字符串

**函数原型：**

- `string& operator+=(const char* str);` //重载+=操作符
- `string& operator+=(const char c);` //重载+=操作符
- `string& operator+=(const string& str);` //重载+=操作符
- `string& append(const char *s);` //把字符串s连接到当前字符串结尾
- `string& append(const char *s, int n);` //把字符串s的前n个字符连接到当前字符串结尾
- `string& append(const string &s);` //同operator+=(const string& str)
- `string& append(const string &s, int pos, int n);`//字符串s中从pos开始的n个字符连接到字符串结尾

**示例：**

```cpp
#include <iostream>
#include <string>
using namespace std;
//string的构造函数
/*
- `string& operator+=(const char* str);` //重载+=操作符
- `string& operator+=(const char c);` //重载+=操作符
- `string& operator+=(const string& str);` //重载+=操作符
- `string& append(const char *s);` //把字符串s连接到当前字符串结尾
- `string& append(const char *s, int n);` //把字符串s的前n个字符连接到当前字符串结尾
- `string& append(const string &s);` //同operator+=(const string& str)
- `string& append(const string &s, int pos, int n);`//字符串s中从pos开始的n个字符连接到字符串结尾
 */
void test01(){
    string s1 = "a";
    s1 += "abc";
    cout<<s1<<endl;
    s1 += 'd';
    cout<<s1<<endl;
    string s2 = "qwer";
    s1 += s2;
    cout<<s1<<endl;
    s1.append("ty");
    cout<<s1<<endl;
    s1.append(" mygame",3);
    cout<<s1<<endl;
    s1.append(s2);
    cout<<s1<<endl;
    s1.append("MYLOVE",2,5);
    cout<<s1<<endl;
}
int main(){
    test01();
    return 0;
}
```

总结：字符串拼接的重载版本很多，初学阶段记住几种即可

#### 3.1.5 string查找和替换

**功能描述：**

- 查找：查找指定字符串是否存在
- 替换：在指定的位置替换字符串

**函数原型：**

- `int find(const string& str, int pos = 0) const;` //查找str第一次出现位置,从pos开始查找
- `int find(const char* s, int pos = 0) const;` //查找s第一次出现位置,从pos开始查找
- `int find(const char* s, int pos, int n) const;` //从pos位置查找s的前n个字符第一次位置
- `int find(const char c, int pos = 0) const;` //查找字符c第一次出现位置
- `int rfind(const string& str, int pos = npos) const;` //查找str最后一次位置,从pos开始查找
- `int rfind(const char* s, int pos = npos) const;` //查找s最后一次出现位置,从pos开始查找
- `int rfind(const char* s, int pos, int n) const;` //从pos查找s的前n个字符最后一次位置
- `int rfind(const char c, int pos = 0) const;` //查找字符c最后一次出现位置
- `string& replace(int pos, int n, const string& str);` //替换从pos开始n个字符为字符串str
- `string& replace(int pos, int n,const char* s);` //替换从pos开始的n个字符为字符串s

**示例：**

```cpp
#include <iostream>
#include <string>
using namespace std;
//string的查找和替换
/*

- `int find(const string& str, int pos = 0) const;` //查找str第一次出现位置,从pos开始查找
- `int find(const char* s, int pos = 0) const;` //查找s第一次出现位置,从pos开始查找
- `int find(const char* s, int pos, int n) const;` //从pos位置查找s的前n个字符第一次位置
- `int find(const char c, int pos = 0) const;` //查找字符c第一次出现位置
- `int rfind(const string& str, int pos = npos) const;` //查找str最后一次位置,从pos开始查找
- `int rfind(const char* s, int pos = npos) const;` //查找s最后一次出现位置,从pos开始查找
- `int rfind(const char* s, int pos, int n) const;` //从pos查找s的前n个字符最后一次位置
- `int rfind(const char c, int pos = 0) const;` //查找字符c最后一次出现位置
- `string& replace(int pos, int n, const string& str);` //替换从pos开始n个字符为字符串str
- `string& replace(int pos, int n,const char* s);` //替换从pos开始的n个字符为字符串s

 */
void test01(){
    //查找
    string s1="abcdefgde";
    //rfind和find的区别
    //rfind从右往左查找 find从左往右查找
    cout<<s1.find("de")<<endl;
    cout<<s1.rfind("de")<<endl;
}
void test02(){
    //替换
    string s1="abcdefgde";
    //从1到3字符替换为1111
    s1.replace(1,3,"1111");
    cout<<s1<<endl;

}
int main(){
    test01();
    test02();
    return 0;
}
```

总结：

- find查找是从左往后，rfind从右往左
- find找到字符串后返回查找的第一个字符位置，找不到返回-1
- replace在替换时，要指定从哪个位置起，多少个字符，替换成什么样的字符串

#### 3.1.6 string字符串比较

**功能描述：**

- 字符串之间的比较

**比较方式：**

- 字符串比较是按字符的ASCII码进行对比

= 返回 0

\> 返回 1

< 返回 -1

**函数原型：**

- `int compare(const string &s) const;` //与字符串s比较
- `int compare(const char *s) const;` //与字符串s比较

**示例：**

```cpp
#include <iostream>
#include <string>
using namespace std;
//string的比较操作
/*

- `int compare(const string &s) const;` //与字符串s比较
- `int compare(const char *s) const;` //与字符串s比较

 */
void test01(){
    string s1 = "hello";
    string s2 = "hello";
    cout<<"= ->"<<s1.compare(s2)<<endl;
    s1 = "xello";
    cout<<"> ->"<<s1.compare(s2)<<endl;
    s1 = "hello";
    s2 = "xello";
    cout<<"< ->"<<s1.compare(s2)<<endl;

}
void test02(){
    //替换
    string s1="abcdefgde";
    //从1到3字符替换为1111
    s1.replace(1,3,"1111");
    cout<<s1<<endl;

}
int main(){
    test01();
//    test02();
    return 0;
}
```

总结：字符串对比主要是用于比较两个字符串是否相等，判断谁大谁小的意义并不是很大

#### 3.1.7 string字符存取

string中单个字符存取方式有两种

- `char& operator[](int n);` //通过[]方式取字符
- `char& at(int n);` //通过at方法获取字符

**示例：**

```cpp
#include <iostream>
#include <string>
using namespace std;
//string的字符存取
void test01(){
    string s1 = "hello";
//    cout<<s1<<endl;
    //通过[]访问单个字符
    for (int i = 0; i < s1.size(); ++i) {
        cout<<s1[i]<<" ";
    }
    cout<<endl;
    // 通过at方式访问单个字符
    for (int i = 0; i < s1.size(); ++i) {
        cout<<s1.at(i)<<" ";
    }
    cout<<endl;

    //修改单个字符
    s1[0] = 'x';
    cout<<s1<<endl;
    s1.at(1) = 'w';
    cout<<s1<<endl;
}


int main(){
    test01();
//    test02();
    return 0;
}
```

总结：string字符串中单个字符存取有两种方式，利用 [ ] 或 at

#### 3.1.8 string插入和删除

**功能描述：**

- 对string字符串进行插入和删除字符操作

**函数原型：**

- `string& insert(int pos, const char* s);` //插入字符串
- `string& insert(int pos, const string& str);` //插入字符串
- `string& insert(int pos, int n, char c);` //在指定位置插入n个字符c
- `string& erase(int pos, int n = npos);` //删除从Pos开始的n个字符

**示例：**

```cpp
#include <iostream>
#include <string>
using namespace std;
//string的插入和删除
void test01(){
    string s1 = "hello";
    //插入
    s1.insert(1,"111");
    cout<<s1<<endl;
    //删除
    s1.erase(1,3);
    cout<<s1<<endl;
}


int main(){
    test01();

    return 0;
}
```

**总结**：插入和删除的起始下标都是从0开始

#### 3.1.9 string子串

**功能描述：**

- 从字符串中获取想要的子串

**函数原型：**

- `string substr(int pos = 0, int n = npos) const;` //返回由pos开始的n个字符组成的字符串

**示例：**

```cpp
#include <iostream>
#include <string>
using namespace std;
//string的子串
void test01(){
    string s1 = "hello";
    string substr = s1.substr(1,3);
    cout<<substr<<endl;
}
//实用操作
void test02(){
    string email = "ZhangSan@qq.com";
    int pos = email.find("@");
    string userName = email.substr(0,pos);
    cout<<userName<<endl;
}

int main(){
    test02();
    return 0;
}
```

**总结**：灵活的运用求子串功能，可以在实际开发中获取有效的信息



### 3.2 vector容器

#### 3.2.1 vector基本概念

**功能：**

- vector数据结构和**数组非常相似**，也称为**单端数组**

**vector与普通数组区别：**

- 不同之处在于数组是静态空间，而vector可以**动态扩展**

**动态扩展：**

- 并不是在原空间之后续接新空间，而是找更大的内存空间，然后将原数据拷贝新空间，释放原空间

![在这里插入图片描述](D:\JYH\mydaily\PublicNoteBooks\c++提高编程.assets\20210525131033234.jpg)

- vector容器的迭代器是支持随机访问的迭代器



#### 3.2.2 vector构造函数

**功能描述：**

- 创建vector容器

**函数原型：**

- `vector<T> v;` //采用模板实现类实现，默认构造函数
- `vector(v.begin(), v.end());` //将v[begin(), end())区间中的元素拷贝给本身。
- `vector(n, elem);` //构造函数将n个elem拷贝给本身。
- `vector(const vector &vec);` //拷贝构造函数。

**示例：**

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
void printVector(vector<int> &v){
    for(vector<int>::iterator it = v.begin();it!=v.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}
//vector构造函数
void test01(){
    //默认构造
    vector<int> v1;
    for (int i = 0; i < 10; ++i) {
        v1.push_back(i);
    }
    printVector(v1);
    //通过区间方式进行构造
    vector<int> v2(v1.begin(),v1.end());
    printVector(v2);
    //n个elem方式构造
    vector<int> v3(6,99);
    printVector(v3);
    //拷贝构造
    vector<int> v4(v3);
    printVector(v4);
}
int main(){
    test01();
    return 0;
}
```

**总结**：vector的多种构造方式没有可比性，灵活使用即可



#### 3.2.3 vector赋值操作

**功能描述：**

- 给vector容器进行赋值

**函数原型：**

- `vector& operator=(const vector &vec);`//重载等号操作符
- `assign(beg, end);` //将[beg, end)区间中的数据拷贝赋值给本身。
- `assign(n, elem);` //将n个elem拷贝赋值给本身。

**示例：**

```cpp
//
// Created by JSQ on 2021/7/8.
//
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
//vector赋值操作
void printVector(vector<int> &v){
    for(vector<int>::iterator it = v.begin();it!=v.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}

void test01(){
    //默认构造
    vector<int> v1;
    for (int i = 0; i < 10; ++i) {
        v1.push_back(i);
    }
    printVector(v1);
    //赋值 =
    vector<int> v2;
    v2 = v1;
    printVector(v2);
    //assign
    vector<int> v3;
    v3.assign(v1.begin(),v1.end());
    printVector(v3);

    vector<int> v4;
    v4.assign(10,100);
    printVector(v4);
}
int main(){
    test01();
    return 0;
}
```

总结： vector赋值方式比较简单，使用operator=，或者assign都可以



#### 3.2.4 vector容量和大小

**功能描述：**

- 对vector容器的容量和大小操作

**函数原型：**

- `empty();` //判断容器是否为空

- `capacity();` //容器的容量

- `size();` //返回容器中元素的个数

- `resize(int num);` //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。

  //如果容器变短，则末尾超出容器长度的元素被删除。

- `resize(int num, elem);` //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。

  //如果容器变短，则末尾超出容器长度的元素被删除

**示例：**

```cpp
//
// Created by JSQ on 2021/7/8.
//
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
//vector容量和大小
void printVector(vector<int> &v){
    for(vector<int>::iterator it = v.begin();it!=v.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}

void test01(){
    //默认构造
    vector<int> v1;
    for (int i = 0; i < 10; ++i) {
        v1.push_back(i);
    }
    printVector(v1);
    //判断是否为空
    if(v1.empty()){
        cout<<"empty"<<endl;
    } else{
        cout<<"size:"<<v1.size()<<endl;
        cout<<"capacity:"<<v1.capacity()<<endl;
    }
    //重新指定大小
    v1.resize(15,100);//利用重载版本，指定填充值
    printVector(v1);//重新指定过长，默认零填充
    v1.resize(5);//利用重载版本，指定填充值
    printVector(v1);//重新指定更短，删除超出的部分
}
int main(){
    test01();
    return 0;
}
```

总结：

- 判断是否为空 — empty
- 返回元素个数 — size
- 返回容器容量 — capacity
- 重新指定大小 — resize



#### 3.2.5 vector插入和删除

**功能描述：**

- 对vector容器进行插入、删除操作

**函数原型：**

- `push_back(ele);` //尾部插入元素ele
- `pop_back();` //删除最后一个元素
- `insert(const_iterator pos, ele);` //迭代器指向位置pos插入元素ele
- `insert(const_iterator pos, int count,ele);`//迭代器指向位置pos插入count个元素ele
- `erase(const_iterator pos);` //删除迭代器指向的元素
- `erase(const_iterator start, const_iterator end);`//删除迭代器从start到end之间的元素
- `clear();` //删除容器中所有元素

**示例：**

```cpp
//
// Created by JSQ on 2021/7/8.
//
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
//vector插入和删除
/*

- `push_back(ele);` //尾部插入元素ele
- `pop_back();` //删除最后一个元素
- `insert(const_iterator pos, ele);` //迭代器指向位置pos插入元素ele
- `insert(const_iterator pos, int count,ele);`//迭代器指向位置pos插入count个元素ele
- `erase(const_iterator pos);` //删除迭代器指向的元素
- `erase(const_iterator start, const_iterator end);`//删除迭代器从start到end之间的元素
- `clear();` //删除容器中所有元素

*/

void printVector(vector<int> &v){
    for(vector<int>::iterator it = v.begin();it!=v.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}

void test01(){
    //默认构造
    vector<int> v1;
    for (int i = 0; i < 10; ++i) {
        v1.push_back(i*10);//尾插法
    }
    printVector(v1);
    //尾删法
    v1.pop_back();
    printVector(v1);
    //插入-第一个参数是迭代器
    v1.insert(v1.begin()+1,55);
    printVector(v1);
    v1.insert(v1.begin()+5,5,33);
    printVector(v1);

    // 删除-参数也是迭代器
    v1.erase(v1.begin()+3);
    printVector(v1);

    v1.erase(v1.begin()+5,v1.end()-5);
    printVector(v1);
    //清空
//    v1.clear();
    v1.erase(v1.begin(),v1.end());
    printVector(v1);
}
int main(){
    test01();
    return 0;
}
```

总结：

- 尾插 — push_back
- 尾删 — pop_back
- 插入 — insert (位置迭代器)
- 删除 — erase （位置迭代器）
- 清空 — clear



#### 3.2.6 vector数据存取

**功能描述：**

- 对vector中的数据的存取操作

**函数原型：**

- `at(int idx);` //返回索引idx所指的数据
- `operator[];` //返回索引idx所指的数据
- `front();` //返回容器中第一个数据元素
- `back();` //返回容器中最后一个数据元素

**示例：**

```cpp
//
// Created by JSQ on 2021/7/9.
//
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
//vector数据存取


void printVector(vector<int> &v){
//    for(vector<int>::iterator it = v.begin();it!=v.end();it++){
//        cout<<*it<<" ";
//    }


    for (int i = 0; i < v.size(); ++i) {
        //利用[]方式访问数组中元素
//        cout<<v[i]<<" ";
        //利用at方式访问数组中元素
        cout<<v.at(i)<<" ";
    }
    cout<<endl;
}

void test01(){
    vector<int> v1;
    for (int i = 0; i < 10; ++i) {
        v1.push_back(i);
    }
    printVector(v1);
    cout<<"first:"<<v1.front()<<endl;
    cout<<"last:"<<v1.back()<<endl;
}
int main(){
    test01();
    return 0;
}
```

总结：

- 除了用迭代器获取vector容器中元素，[ ]和at也可以
- front返回容器第一个元素
- back返回容器最后一个元素



#### 3.2.7 vector互换容器

**功能描述：**

- 实现两个容器内元素进行互换

**函数原型：**

- `swap(vec);` // 将vec与本身的元素互换

**示例：**

```cpp
//
// Created by JSQ on 2021/7/9.
//
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
//vector的互换
void printVector(vector<int> &v){
    for(vector<int>::iterator it = v.begin();it!=v.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}
//基本使用
void test01(){
    vector<int> v1;
    for (int i = 0; i < 10; ++i) {
        v1.push_back(i);
    }
    cout<<"交换前:"<<endl;
    printVector(v1);
    vector<int> v2;
    for (int i = 10; i > 0; --i) {
        v2.push_back(i);
    }
    printVector(v2);
    cout<<"交换后:"<<endl;
    v1.swap(v2);
    printVector(v1);
    printVector(v2);
}
//实际用途 巧用swap可以收缩内存空间
void test02(){
    vector<int> v1;
    for (int i = 0; i < 100000; ++i) {
        v1.push_back(i);
    }
    cout<<"v1 capacity:"<<v1.capacity()<<endl;
    cout<<"v1 size:"<<v1.size()<<endl;

    v1.resize(3);//重新指定大小
    cout<<"v1 capacity:"<<v1.capacity()<<endl;
    cout<<"v1 size:"<<v1.size()<<endl;
    //巧用swap可以收缩内存空间
    vector<int>(v1).swap(v1);
    //vector<int>(v1) 匿名函数，在当前行执行完之后系统会主动回收当前匿名函数占用内存
    //.swap(v1) 容器交换
    cout<<"v1 capacity:"<<v1.capacity()<<endl;
    cout<<"v1 size:"<<v1.size()<<endl;
}
int main(){
    test02();
    return 0;
}
```

总结：swap可以使两个容器互换，可以达到实用的收缩内存效果



#### 3.2.8 vector预留空间

**功能描述：**

- 减少vector在动态扩展容量时的扩展次数

**函数原型：**

- `reserve(int len);`//容器预留len个元素长度，预留位置不初始化，元素不可访问。

  

**示例：**

```cpp
//
// Created by JSQ on 2021/7/9.
//
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
//vector的预留空间-减少动态扩展的次数
void test01(){
    vector<int> v1;
    //利用reserve预留空间操作
    v1.reserve(100000);
    int num = 0;//统计开辟次数
    int *p = NULL;
    for (int i = 0; i < 100000; ++i) {
        v1.push_back(i);
        if(p != &v1[0]){
            p = &v1[0];
            num ++;
        }
    }
    cout<<"num="<<num<<endl;
}

int main(){
    test01();
    return 0;
}
```

总结：如果数据量较大，可以一开始利用reserve预留空间



### 3.3 deque容器

#### 3.3.1 deque容器基本概念

**功能：**

- 双端数组，可以对头端进行插入删除操作

**deque与vector区别：**

- vector对于头部的插入删除效率低，数据量越大，效率越低
- deque相对而言，对头部的插入删除速度回比vector快
- vector访问元素时的速度会比deque快,这和两者内部实现有关

![在这里插入图片描述](D:\JYH\mydaily\PublicNoteBooks\c++提高编程.assets\20210525131105613.jpg)

deque内部工作原理:

deque内部有个**中控器**，维护每段缓冲区中的内容，缓冲区中存放真实数据

中控器维护的是每个缓冲区的地址，使得使用deque时像一片连续的内存空间

![在这里插入图片描述](D:\JYH\mydaily\PublicNoteBooks\c++提高编程.assets\20210525131125669.jpg)

- deque容器的迭代器也是支持随机访问的



#### 3.3.2 deque构造函数

**功能描述：**

- deque容器构造

**函数原型：**

- `deque<T>` deqT; //默认构造形式
- `deque(beg, end);` //构造函数将[beg, end)区间中的元素拷贝给本身。
- `deque(n, elem);` //构造函数将n个elem拷贝给本身。
- `deque(const deque &deq);` //拷贝构造函数

**示例：**

```cpp
#include <iostream>
#include <string>
#include <deque>
using namespace std;
//deque构造函数
void printDeque(const deque<int> &d1){
    for(deque<int>::const_iterator it = d1.begin(); it!=d1.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}
void test01(){
    deque<int> d1;
    for (int i = 0; i < 10; ++i) {
        d1.push_back(i);//尾插
    }
    printDeque(d1);
    deque<int> d2(d1);
    printDeque(d2);
    deque<int> d3(d1.begin(),d1.end());
    printDeque(d3);
    deque<int> d4(10,100);
    printDeque(d4);
}
int main(){
    test01();
    return 0;
}
```

**总结：**deque容器和vector容器的构造方式几乎一致，灵活使用即可



#### 3.3.3 deque赋值操作

**功能描述：**

- 给deque容器进行赋值

**函数原型：**

- `deque& operator=(const deque &deq);` //重载等号操作符
- `assign(beg, end);` //将[beg, end)区间中的数据拷贝赋值给本身。
- `assign(n, elem);` //将n个elem拷贝赋值给本身。

**示例：**

```cpp
//
// Created by JSQ on 2021/7/9.
//
#include <iostream>
#include <string>
#include <deque>
using namespace std;
//deque赋值操作
void printDeque(const deque<int> &d1){
    for(deque<int>::const_iterator it = d1.begin(); it!=d1.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}
void test01(){
    deque<int> d1;
    for (int i = 0; i < 10; ++i) {
        d1.push_back(i);//尾插
    }
    printDeque(d1);
    // =
    deque<int> d2;
    d2 = d1;
    printDeque(d2);
    //assign
    deque<int> d3;
    d3.assign(d1.begin(),d1.end());
    printDeque(d3);
    deque<int> d4;
    d4.assign(10,100);
    printDeque(d4);
}
int main(){
    test01();
    return 0;
}

```

总结：deque赋值操作也与vector相同，需熟练掌握



#### 3.3.4 deque大小操作

**功能描述：**

- 对deque容器的大小进行操作

**函数原型：**

- `deque.empty();` //判断容器是否为空

- `deque.size();` //返回容器中元素的个数

- `deque.resize(num);` //重新指定容器的长度为num,若容器变长，则以默认值填充新位置。

  //如果容器变短，则末尾超出容器长度的元素被删除。

- `deque.resize(num, elem);` //重新指定容器的长度为num,若容器变长，则以elem值填充新位置。

  //如果容器变短，则末尾超出容器长度的元素被删除。

  

**示例：**

```cpp
//
// Created by JSQ on 2021/7/9.
//
#include <iostream>
#include <string>
#include <deque>
using namespace std;
//deque容器的大小操作
void printDeque(const deque<int> &d1){
    for(deque<int>::const_iterator it = d1.begin(); it!=d1.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}
void test01(){
    deque<int> d1;
    for (int i = 0; i < 10; ++i) {
        d1.push_back(i);//尾插
    }
    printDeque(d1);
    if(d1.empty()){
        cout<<"empty"<<endl;
    }else{
        cout<<"size:"<<d1.size()<<endl;
    }
    //重新指定大小
//    d1.resize(15);//默认零填充
    d1.resize(15,1);
    printDeque(d1);
    d1.resize(5);
    printDeque(d1);
}
int main(){
    test01();
    return 0;
}
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";

	}
	cout << endl;
}

//大小操作
void test01()
{
	deque<int> d1;
	for (int i = 0; i < 10; i++)
	{
		d1.push_back(i);
	}
	printDeque(d1);

	//判断容器是否为空
	if (d1.empty()) {
		cout << "d1为空!" << endl;
	}
	else {
		cout << "d1不为空!" << endl;
		//统计大小
		cout << "d1的大小为：" << d1.size() << endl;
	}

	//重新指定大小
	d1.resize(15, 1);
	printDeque(d1);

	d1.resize(5);
	printDeque(d1);
}

int main() {

	test01();

	system("pause");

	return 0;
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

总结：

- deque没有容量的概念
- 判断是否为空 — empty
- 返回元素个数 — size
- 重新指定个数 — resize



#### 3.3.5 deque 插入和删除

**功能描述：**

- 向deque容器中插入和删除数据

**函数原型：**

两端插入操作：

- `push_back(elem);` //在容器尾部添加一个数据
- `push_front(elem);` //在容器头部插入一个数据
- `pop_back();` //删除容器最后一个数据
- `pop_front();` //删除容器第一个数据

指定位置操作：

- `insert(pos,elem);` //在pos位置插入一个elem元素的拷贝，返回新数据的位置。

- `insert(pos,n,elem);` //在pos位置插入n个elem数据，无返回值。

- `insert(pos,beg,end);` //在pos位置插入[beg,end)区间的数据，无返回值。

- `clear();` //清空容器的所有数据

- `erase(beg,end);` //删除[beg,end)区间的数据，返回下一个数据的位置。

- `erase(pos);` //删除pos位置的数据，返回下一个数据的位置。

  

  

**示例：**

```cpp
//
// Created by JSQ on 2021/7/9.
//
#include <iostream>
#include <string>
#include <deque>
using namespace std;
//deque容器的插入与删除
void printDeque(const deque<int> &d1){
    for(deque<int>::const_iterator it = d1.begin(); it!=d1.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}
void test01(){
    deque<int> d1;
    for (int i = 0; i < 10; ++i) {
        d1.push_back(i);//尾插
    }
    //头插
    d1.push_front(100);
    d1.push_front(200);
    printDeque(d1);
    //尾删
    d1.pop_back();
    //头删
    d1.pop_front();
    printDeque(d1);
    //指定插入
    d1.insert(d1.begin()+1,1000);
    printDeque(d1);
    d1.insert(d1.begin()+3,2,3000);
    printDeque(d1);
    deque<int>d2;
    d2.push_back(10);
    d2.push_back(20);
    d2.push_back(30);
    d1.insert(d1.begin()+1,d2.begin(),d2.end());//区间插入
    printDeque(d1);
    //删除
    d1.erase(d1.begin());
    printDeque(d1);
    d1.erase(d1.begin()+2,d1.end()-5);//区间删除
    printDeque(d1);
    d1.clear();
//    d1.erase(d1.begin(),d1.end());
    printDeque(d1);
}
int main(){
    test01();
    return 0;
}
```

总结：

- 插入和删除提供的位置是迭代器！
- 尾插 — push_back
- 尾删 — pop_back
- 头插 — push_front
- 头删 — pop_front



#### 3.3.6 deque 数据存取

**功能描述：**

- 对deque 中的数据的存取操作

**函数原型：**

- `at(int idx);` //返回索引idx所指的数据
- `operator[];` //返回索引idx所指的数据
- `front();` //返回容器中第一个数据元素
- `back();` //返回容器中最后一个数据元素

**示例：**

```cpp
//
// Created by JSQ on 2021/7/9.
//
#include <iostream>
#include <string>
#include <deque>
using namespace std;
//deque容器的数据存取
void printDeque(const deque<int> &d1){
//    for(deque<int>::const_iterator it = d1.begin(); it!=d1.end();it++){
//        cout<<*it<<" ";
//    }
    //[]访问
//    for (int i = 0; i < d1.size(); ++i) {
//        cout<<d1[i]<<" ";
//    }
    //at访问
    for (int i = 0; i < d1.size(); ++i) {
        cout<<d1.at(i)<<" ";
    }
    cout<<endl;
}
void test01(){
    deque<int> d1;
    for (int i = 0; i < 10; ++i) {
        d1.push_back(i*10);//尾插
    }
    printDeque(d1);
    cout<<"front:"<<d1.front()<<endl;
    cout<<"back:"<<d1.back()<<endl;
}
int main(){
    test01();
    return 0;
}
```

总结：

- 除了用迭代器获取deque容器中元素，[ ]和at也可以
- front返回容器第一个元素
- back返回容器最后一个元素



#### 3.3.7 deque 排序

**功能描述：**

- 利用算法实现对deque容器进行排序

**算法：**

- `sort(iterator beg, iterator end)` //对beg和end区间内元素进行排序

**示例：**

```cpp
//
// Created by JSQ on 2021/7/9.
//
#include <iostream>
#include <string>
#include <deque>
#include <algorithm>
using namespace std;
//deque容器的数据存取
void printDeque(const deque<int> &d1){
    for(deque<int>::const_iterator it = d1.begin(); it!=d1.end();it++){
        cout<<*it<<" ";
    }
    cout<<endl;
}
void test01(){
    deque<int> d1;
    d1.push_back(10);
    d1.push_back(30);
    d1.push_back(20);
    d1.push_front(40);
    d1.push_front(15);
    d1.push_front(35);
    printDeque(d1);
    //默认升序 
    // 对于支持随机访问的迭代器的容器都可以用sort算法排序
    sort(d1.begin(), d1.end());
    printDeque(d1);
}
int main(){
    test01();
    return 0;
}
```

总结：sort算法非常实用，使用时包含头文件 algorithm即可



### 3.4 案例-评委打分

#### 3.4.1 案例描述

有5名选手：选手ABCDE，10个评委分别对每一名选手打分，去除最高分，去除评委中最低分，取平均分。

#### 3.4.2 实现步骤

1. 创建五名选手，放到vector中
2. 遍历vector容器，取出来每一个选手，执行for循环，可以把10个评分打分存到deque容器中
3. sort算法对deque容器中分数排序，去除最高和最低分
4. deque容器遍历一遍，累加总分
5. 获取平均分

**示例代码：**

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <deque>
#include <algorithm>
#include <ctime>
using namespace std;
//五名选手
//10名评委
//选手类
class Person{
public:
    Person(string n, int s){
        this->name = n;
        this->score = s;
    }
    string name;
    int score;//平均分
};
void createPerson(vector<Person> &v){
    string nameSeed = "ABCDE";
    for (int i = 0; i < 5; ++i) {
        string name = "Player";
        name += nameSeed[i];
        int score = 0;
        Person p(name,score);
        //将创建的Person对象放入容器
        v.push_back(p);
    }
}
void showPerson(vector<Person> &v){
    for(vector<Person>::iterator it = v.begin();it!=v.end();it++){
        cout<<(*it).name<<", "<<(*it).score<<endl;
    }
}
void setScore(vector<Person> &v){

    for(vector<Person>::iterator it = v.begin();it!=v.end();it++){
        //将评委的分数放入deque容器中
        deque<int>d;
        for (int i = 0; i < 10; ++i) {
            int score = rand()%41+60;//60~100
            d.push_back(score);
        }
        sort(d.begin(), d.end());
        //去除最高最低分
        d.pop_back();
        d.pop_front();
        //取平均分
        int sum = 0;
        for(deque<int>::iterator dit = d.begin();dit!=d.end();dit++){
            sum += (*dit);
        }
        int avg = sum/ d.size();
        //将平均分放入选手中
        it->score =avg;
    }
}
int main() {
    //随机数种子
    srand((unsigned  int)time(NULL));
    //创建五名选手
    vector<Person>v;//存放选手的容器
    createPerson(v);
//    showPerson(v);
    //打分
    setScore(v);
    showPerson(v);
    //显示得分
    return 0;
}
```

**总结：** 选取不同的容器操作数据，可以提升代码的效率



### 3.5 stack容器

#### 3.5.1 stack 基本概念

**概念：\**stack是一种\**先进后出**(First In Last Out,FILO)的数据结构，它只有一个出口

![在这里插入图片描述](D:\JYH\mydaily\PublicNoteBooks\c++提高编程.assets\20210525131158267.jpg)

栈中只有顶端的元素才可以被外界使用，因此栈不允许有遍历行为

栈中进入数据称为 — **入栈** `push`

栈中弹出数据称为 — **出栈** `pop`

生活中的栈：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021052513121869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0F1Z2Vuc3Rlcm5fUVhM,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210525131233741.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0F1Z2Vuc3Rlcm5fUVhM,size_16,color_FFFFFF,t_70#pic_center)



#### 3.5.2 stack 常用接口

功能描述：栈容器常用的对外接口

构造函数：

- `stack<T> stk;` //stack采用模板类实现， stack对象的默认构造形式
- `stack(const stack &stk);` //拷贝构造函数

赋值操作：

- `stack& operator=(const stack &stk);` //重载等号操作符

数据存取：

- `push(elem);` //向栈顶添加元素
- `pop();` //从栈顶移除第一个元素
- `top();` //返回栈顶元素

大小操作：

- `empty();` //判断堆栈是否为空
- `size();` //返回栈的大小

**示例：**

```cpp
//
// Created by JSQ on 2021/7/9.
//
#include <iostream>
#include <string>
#include <stack>
using namespace std;
//栈stack容器
void test01(){
    //FILO
    stack<int>s;
    //入栈
    s.push(10);
    s.push(20);
    s.push(30);
    s.push(40);
    s.push(50);
    cout<<s.size()<<endl;
    //只要栈不为空，查看栈顶，并执行出栈操作
    while (!s.empty()){
        cout<<s.top()<<endl;
        s.pop();
    }
    cout<<s.size();
}
int main(){
    test01();
    return 0;
}

```

总结：

- 入栈 — push
- 出栈 — pop
- 返回栈顶 — top
- 判断栈是否为空 — empty
- 返回栈大小 — size



### 3.6 queue 容器

#### 3.6.1 queue 基本概念

**概念：\**Queue是一种\**先进先出**(First In First Out,FIFO)的数据结构，它有两个出口

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210525131255672.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0F1Z2Vuc3Rlcm5fUVhM,size_16,color_FFFFFF,t_70#pic_center)

队列容器允许从一端新增元素，从另一端移除元素

队列中只有队头和队尾才可以被外界使用，因此队列不允许有遍历行为

队列中进数据称为 — **入队** `push`

队列中出数据称为 — **出队** `pop`

生活中的队列：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210525131310596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0F1Z2Vuc3Rlcm5fUVhM,size_16,color_FFFFFF,t_70#pic_center)



#### 3.6.2 queue 常用接口

功能描述：栈容器常用的对外接口

构造函数：

- `queue<T> que;` //queue采用模板类实现，queue对象的默认构造形式
- `queue(const queue &que);` //拷贝构造函数

赋值操作：

- `queue& operator=(const queue &que);` //重载等号操作符

数据存取：

- `push(elem);` //往队尾添加元素
- `pop();` //从队头移除第一个元素
- `back();` //返回最后一个元素
- `front();` //返回第一个元素

大小操作：

- `empty();` //判断堆栈是否为空
- `size();` //返回栈的大小

**示例：**

```cpp
#include <iostream>
#include <string>
#include <queue>
using namespace std;
//队列
class Person{
public:
    Person(string n, int a){
        this->name = n;
        this->age = a;
    }
    string name;
    int age;
};
void test01(){
    queue<Person>q;
    Person p1("Zhangsan",20);
    Person p2("LiSi",30);
    Person p3("Wangming",40);

    //入队
    q.push(p1);
    q.push(p2);
    q.push(p3);
    cout<<q.size()<<endl;
    //判断只要队列不为空，查看队头、队尾、出队
    while (!q.empty()){
        //查看队头
        cout<<q.front().name<<":"<<q.front().age<<endl;
        //查看队尾
        cout<<q.back().name<<":"<<q.back().age<<endl;

        //
        q.pop();
    }
    cout<<q.size()<<endl;
}
int main() {
    test01();
    return 0;
}
```

总结：

- 入队 — push
- 出队 — pop
- 返回队头元素 — front
- 返回队尾元素 — back
- 判断队是否为空 — empty
- 返回队列大小 — size



### 3.7 list容器

#### 3.7.1 list基本概念

功能：**将数据进行链式存储**

**链表**（list）是一种物理存储单元上非连续的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的

链表的组成：链表由一系列**结点**组成

结点的组成：一个是存储数据元素的**数据域**，另一个是存储下一个结点地址的**指针域**

STL中的链表是一个双向循环链表

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021052513133556.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0F1Z2Vuc3Rlcm5fUVhM,size_16,color_FFFFFF,t_70#pic_center)

由于链表的存储方式并不是连续的内存空间，因此链表list中的迭代器只支持前移和后移，属于**双向迭代器**

list的优点：

- 采用动态存储分配，不会造成内存浪费和溢出
- 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

list的缺点：

- 链表灵活，但是空间(指针域) 和 时间（遍历）额外耗费较大

List有一个重要的性质，插入操作和删除操作都不会造成原有list迭代器的失效，这在vector是不成立的。

总结：STL中**List和vector是两个最常被使用的容器**，各有优缺点

#### 3.7.2 list构造函数

**功能描述：**

- 创建list容器

**函数原型：**

- `list<T> lst;` //list采用采用模板类实现,对象的默认构造形式：
- `list(beg,end);` //构造函数将[beg, end)区间中的元素拷贝给本身。
- `list(n,elem);` //构造函数将n个elem拷贝给本身。
- `list(const list &lst);` //拷贝构造函数。

**示例：**

```cpp
#include <iostream>
#include <string>
#include <list>
using namespace std;
//链表构造函数
void printList(const list<int> &lst){
    for(list<int>::const_iterator it = lst.begin();it!=lst.end();it++){
        cout<< *it <<" ";
    }
    cout<<endl;
}
void test01(){
    list<int>lst;
    lst.push_back(10);
    lst.push_back(20);
    lst.push_back(30);
    lst.push_back(40);
    lst.push_back(50);

    printList(lst);

    //区间构造
    list<int> lst2(lst.begin(),lst.end());
    printList(lst2);
    //拷贝构造
    list<int> lst3(lst2);
    printList(lst3);
    //n个elm
    list<int> lst4(10,1000);
    printList(lst4);
}
int main() {
    test01();
    return 0;
}

```

总结：list构造方式同其他几个STL常用容器，熟练掌握即可



#### 3.7.3 list 赋值和交换

**功能描述：**

- 给list容器进行赋值，以及交换list容器

**函数原型：**

- `assign(beg, end);` //将[beg, end)区间中的数据拷贝赋值给本身。
- `assign(n, elem);` //将n个elem拷贝赋值给本身。
- `list& operator=(const list &lst);` //重载等号操作符
- `swap(lst);` //将lst与本身的元素互换。

**示例：**

```cpp
#include <iostream>
#include <string>
#include <list>
using namespace std;
//链表赋值与交换
void printList(const list<int> &lst){
    for(list<int>::const_iterator it = lst.begin();it!=lst.end();it++){
        cout<< *it <<" ";
    }
    cout<<endl;
}
void test01(){
    list<int>lst;
    lst.push_back(10);
    lst.push_back(20);
    lst.push_back(30);
    lst.push_back(40);
    lst.push_back(50);
    printList(lst);

    list<int> lst2;
    lst2 = lst;
    printList(lst2);

    list<int> lst3;
    lst3.assign(lst2.begin(), lst2.end());
    printList(lst3);

    list<int> lst4;
    lst4.assign(10, 100);
    printList(lst4);

    //交换
    lst.swap(lst4);
    printList(lst);
    printList(lst4);
}
int main() {
    test01();
    return 0;
}
```

总结：list赋值和交换操作能够灵活运用即可

#### 3.7.4 list 大小操作

**功能描述：**

- 对list容器的大小进行操作

**函数原型：**

- `size();` //返回容器中元素的个数

- `empty();` //判断容器是否为空

- `resize(num);` //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。

  //如果容器变短，则末尾超出容器长度的元素被删除。

- `resize(num, elem);` //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。

  ​										//如果容器变短，则末尾超出容器长度的元素被删除。

  

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <list>
using namespace std;
//链表大小操作
void printList(const list<int> &lst){
    for(list<int>::const_iterator it = lst.begin();it!=lst.end();it++){
        cout<< *it <<" ";
    }
    cout<<endl;
}
void test01(){
    list<int>lst;
    lst.push_back(10);
    lst.push_back(20);
    lst.push_back(30);
    lst.push_back(40);
    lst.push_back(50);
    printList(lst);
    if(lst.empty()){
        cout<<"empty！"<<endl;
    } else{
        cout<<"size:"<<lst.size()<<endl;
    }
    lst.resize(10,200);
    printList(lst);
    lst.resize(2);
    printList(lst);
}
int main() {
    test01();
    return 0;
}

```

总结：

- 判断是否为空 — empty
- 返回元素个数 — size
- 重新指定个数 — resize



#### 3.7.5 list 插入和删除

**功能描述：**

- 对list容器进行数据的插入和删除

**函数原型：**

- `push_back(elem)`;//在容器尾部加入一个元素
- `pop_back()`;//删除容器中最后一个元素
- `push_front(elem)`;//在容器开头插入一个元素
- `pop_front()`;//从容器开头移除第一个元素
- `insert(pos,elem)`;//在pos位置插elem元素的拷贝，返回新数据的位置。
- `insert(pos,n,elem)`;//在pos位置插入n个elem数据，无返回值。
- `insert(pos,beg,end)`;//在pos位置插入[beg,end)区间的数据，无返回值。
- `clear()`;//移除容器的所有数据
- `erase(beg,end)`;//删除[beg,end)区间的数据，返回下一个数据的位置。
- `erase(pos)`;//删除pos位置的数据，返回下一个数据的位置。
- `remove(elem)`;//删除容器中所有与elem值匹配的元素。

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <list>
using namespace std;
//链表插入与删除
void printList(const list<int> &lst){
    for(list<int>::const_iterator it = lst.begin();it!=lst.end();it++){
        cout<< *it <<" ";
    }
    cout<<endl;
}
void test01(){
    list<int>lst;
    lst.push_back(10);
    lst.push_back(20);
    lst.push_back(30);
    lst.push_back(40);
    lst.push_back(50);
    printList(lst);

    lst.push_front(100);
    printList(lst);

    lst.pop_back();
    printList(lst);

    lst.pop_front();
    printList(lst);

    //insert
    lst.insert(++lst.begin(),100);//双向迭代器，只能执行++指向下一个位置
    printList(lst);

    //删除
    lst.erase(++lst.begin());
    printList(lst);
    //移除 所有匹配值
    lst.push_back(20);
    lst.push_back(20);
    lst.push_back(20);
    lst.push_back(20);
    lst.remove(20);
    printList(lst);
}
int main() {
    test01();
    return 0;
}
```

总结：

- 尾插 — push_back
- 尾删 — pop_back
- 头插 — push_front
- 头删 — pop_front
- 插入 — insert
- 删除 — erase
- 移除 — remove
- 清空 — clear



#### 3.7.6 list 数据存取

**功能描述：**

- 对list容器中数据进行存取

**函数原型：**

- `front();` //返回第一个元素。
- `back();` //返回最后一个元素。

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <list>
using namespace std;
//链表存取
void printList(const list<int> &lst){
    for(list<int>::const_iterator it = lst.begin();it!=lst.end();it++){
        cout<< *it <<" ";
    }
    cout<<endl;
}
void test01(){
    list<int>lst;
    lst.push_back(10);
    lst.push_back(20);
    lst.push_back(30);
    lst.push_back(40);
    lst.push_back(50);
    printList(lst);
//    不可以用[]，at方式访问list链表容器中的元素
//    因为其本质是链表，不是用连续线性空间存储数据，迭代器也是不支持随机访问
    cout<<"front:"<<lst.front()<<endl;
    cout<<"back:"<<lst.back()<<endl;
    //验证迭代器是不支持随机访问的
    list<int>::iterator it = lst.begin();
    it++;
    it--;//支持双向
//    it += 1;//不支持随机访问
}
int main() {
    test01();
    return 0;
}
```

总结：

- list容器中不可以通过[]或者at方式访问数据
- 返回第一个元素 — front
- 返回最后一个元素 — back



#### 3.7.7 list 反转和排序

**功能描述：**

- 将容器中的元素反转，以及将容器中的数据进行排序

**函数原型：**

- `reverse();` //反转链表
- `sort();` //链表排序

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <list>
#include <algorithm>
using namespace std;
//list容器的反转和排序
void printList(const list<int> &lst){
    for(list<int>::const_iterator it = lst.begin();it!=lst.end();it++){
        cout<< *it <<" ";
    }
    cout<<endl;
}
bool myCompare(int v1,int v2){
    //降序 ，就让第一个数大于第二个数
    return v1>v2;
}
void test01(){
    list<int>lst;
    lst.push_back(10);
    lst.push_back(20);
    lst.push_front(30);
    lst.push_back(40);
    lst.push_front(50);
    printList(lst);

    lst.reverse();
    printList(lst);
    //sort算法只支持可随机访问的容器，不可用标准算法
    // 不支持随机访问迭代器的容器，内部会提供对应的一些算法
//    sort(lst.begin(), lst.end());
//    printList(lst);//报错
    lst.sort();//默认从小到大
    printList(lst);
    lst.sort(myCompare);//降序排序
    printList(lst);
}
int main() {
    test01();
    return 0;
}
```

总结：

- 反转 — reverse
- 排序 — sort （成员函数）



#### 3.7.8 排序案例

案例描述：将Person自定义数据类型进行排序，Person中属性有姓名、年龄、身高

排序规则：按照年龄进行升序，如果年龄相同按照身高进行降序

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <list>
#include <algorithm>
using namespace std;
//list容器的排序案例
class Person{
public:
    Person(string n, int a, int h){
        this->age = a;
        this->height = h;
        this->name = n;
    }
    string name;
    int age;
    int height;
};
bool ComparePersonAge(Person &p1,Person &p2){
    if(p1.age == p2.age){
        return p1.height<p2.height;
    }
    return p1.age<p2.age;
}
bool ComparePersonHeight(Person &p1,Person &p2){
    if(p1.height==p2.height){
        return p1.age<p2.age;
    }
    return p1.height<p2.height;
}
void printList(const list<Person> &lst){
    for(list<Person>::const_iterator it = lst.begin();it!=lst.end();it++){
        cout<< (*it).name <<","<<(*it).age<<","<<(*it).height<<endl;
    }
    cout<<endl;
}
bool myCompare(int v1,int v2){
    //降序 ，就让第一个数大于第二个数
    return v1>v2;
}
void test01(){
    list<Person>L;
    Person p1("刘备", 35 , 175);
    Person p2("曹操", 45 , 180);
    Person p3("孙权", 40 , 170);
    Person p4("赵云", 25 , 190);
    Person p5("张飞", 35 , 160);
    Person p6("关羽", 35 , 200);

    L.push_back(p1);
    L.push_back(p2);
    L.push_back(p3);
    L.push_back(p4);
    L.push_back(p5);
    L.push_back(p6);
    printList(L);

    cout<<"排序："<<endl;
    L.sort(ComparePersonAge);
    for(list<Person>::const_iterator it = L.begin();it!=L.end();it++){
        cout<< (*it).name <<","<<(*it).age<<","<<(*it).height<<endl;
    }
}
int main() {
    test01();
    return 0;
}
```

总结：

- 对于自定义数据类型，必须要指定排序规则，否则编译器不知道如何进行排序
- 高级排序只是在排序规则上再进行一次逻辑规则制定，并不复杂



### 3.8 set/ multiset 容器

#### 3.8.1 set基本概念

**简介：**

- 所有元素都会在插入时自动被排序

**本质：**

- set/multiset属于**关联式容器**，底层结构是用**二叉树**实现。

**set和multiset区别**：

- set不允许容器中有重复的元素
- multiset允许容器中有重复的元素

#### 3.8.2 set构造和赋值

功能描述：创建set容器以及赋值

构造：

- `set<T> st;` //默认构造函数：
- `set(const set &st);` //拷贝构造函数

赋值：

- `set& operator=(const set &st);` //重载等号操作符

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <set>
using namespace std;
//set容器构造和赋值
// set容器特点：
// 1.所有元素插入时自动被排序
// 2.不允许插入重复值
void printSet(set<int> &s1) {
    for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << (*it) << " ";
    }
    cout << endl;
}
void test01(){
    set<int>s1;
    //只有insert方式插入数据
    s1.insert(10);
    s1.insert(20);
    s1.insert(40);
    s1.insert(30);
    s1.insert(40);
    printSet(s1);

    set<int>s2(s1);
    printSet(s2);

    set<int>s3;
    s3 = s2;
    printSet(s3);
}
int main() {
    test01();
    return 0;
}
```

总结：

- set容器插入数据时用insert
- set容器插入数据的数据会自动排序



#### 3.8.3 set大小和交换

**功能描述：**

- 统计set容器大小以及交换set容器

**函数原型：**

- `size();` //返回容器中元素的数目
- `empty();` //判断容器是否为空
- `swap(st);` //交换两个集合容器

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <set>
using namespace std;
//set容器构造和赋值
// set容器特点：
// 1.所有元素插入时自动被排序
// 2.不允许插入重复值
void printSet(set<int> &s1) {
    for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << (*it) << " ";
    }
    cout << endl;
}
void test01(){
    set<int>s1;
    s1.insert(10);
    s1.insert(20);
    s1.insert(40);
    s1.insert(30);
    s1.insert(40);
    printSet(s1);

    if (s1.empty()){
        cout<<"empty!"<<endl;
    } else{
        cout<<"size:"<<s1.size()<<endl;
    }

    set<int>s2;
    s2.insert(1);
    s2.insert(2);
    s2.insert(4);
    s2.insert(3);
    printSet(s2);

    s1.swap(s2);
    printSet(s1);
    printSet(s2);
}
int main() {
    test01();
    return 0;
}
```

总结：

- 统计大小 — size
- 判断是否为空 — empty
- 交换容器 — swap



#### 3.8.4 set插入和删除

**功能描述：**

- set容器进行插入数据和删除数据

**函数原型：**

- `insert(elem);` //在容器中插入元素。
- `clear();` //清除所有元素
- `erase(pos);` //删除pos迭代器所指的元素，返回下一个元素的迭代器。
- `erase(beg, end);` //删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
- `erase(elem);` //删除容器中值为elem的元素。

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
//set容器插入和删除
#include <iostream>
#include <string>
#include <set>
using namespace std;
void printSet(set<int> &s1) {
    for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << (*it) << " ";
    }
    cout << endl;
}
void test01(){
    set<int>s1;
    s1.insert(10);
    s1.insert(20);
    s1.insert(40);
    s1.insert(30);
    s1.insert(40);
    printSet(s1);

    s1.erase(++s1.begin());
    printSet(s1);
    s1.erase(30);
    printSet(s1);

//    s1.erase(s1.begin(),s1.end());
    s1.clear();
    printSet(s1);

}
int main() {
    test01();
    return 0;
}
```

总结：

- 插入 — insert
- 删除 — erase
- 清空 — clear



#### 3.8.5 set查找和统计

**功能描述：**

- 对set容器进行查找数据以及统计数据

**函数原型：**

- `find(key);` //查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
- `count(key);` //统计key的元素个数

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
//set容器查找和统计
#include <iostream>
#include <string>
#include <set>
using namespace std;
void printSet(set<int> &s1) {
    for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << (*it) << " ";
    }
    cout << endl;
}
void test01(){
    set<int>s1;
    s1.insert(10);
    s1.insert(20);
    s1.insert(40);
    s1.insert(30);
    s1.insert(40);
    printSet(s1);
    set<int>::iterator pos = s1.find(20);
    if(pos!=s1.end()){
        cout<<"find!->"<<*pos<<endl;
    } else{
        cout<<"not find"<<endl;
    }
    int num = s1.count(10);
    cout<<"num:"<<num<<endl;
    multiset<int>ms1;
    ms1.insert(10);
    ms1.insert(20);
    ms1.insert(40);
    ms1.insert(30);
    ms1.insert(40);
    num = ms1.count(40);
    cout<<"num:"<<num<<endl;
}
int main() {
    test01();
    return 0;
}
```

总结：

- 查找 — find （返回的是迭代器）
- 统计 — count （对于set，结果为0或者1）



#### 3.8.6 set和multiset区别

**学习目标：**

- 掌握set和multiset的区别

**区别：**

- set不可以插入重复数据，而multiset可以
- set插入数据的同时会返回插入结果，表示插入是否成功
- multiset不会检测数据，因此可以插入重复数据

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
//set和multiset的区别
#include <iostream>
#include <string>
#include <set>
using namespace std;
void printSet(set<int> &s1) {
    for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << (*it) << " ";
    }
    cout << endl;
}
void printMultiSet(multiset<int> &s1) {
    for (multiset<int>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << (*it) << " ";
    }
    cout << endl;
}
void test01(){
    set<int>s1;
    s1.insert(10);
    s1.insert(20);
    s1.insert(40);
    s1.insert(30);
    pair<set<int>::iterator,bool> ret = s1.insert(40);
    if(ret.second){
        cout<<"insert Successfully"<<endl;
    } else{
        cout<<"insert Failed"<<endl;
    }
    printSet(s1);


    //允许重复插入 无重复检测
    multiset<int>ms1;
    ms1.insert(10);
    ms1.insert(20);
    ms1.insert(40);
    ms1.insert(30);
    ms1.insert(40);
    printMultiSet(ms1);
}
int main() {
    test01();
    return 0;
}
```

总结：

- 如果不允许插入重复数据可以利用set
- 如果需要插入重复数据利用multiset



#### 3.8.7 pair对组创建

**功能描述：**

- 成对出现的数据，利用对组可以返回两个数据

**两种创建方式：**

- `pair<type, type> p ( value1, value2 );`
- `pair<type, type> p = make_pair( value1, value2 );`

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
//pair对组创建
#include <iostream>
#include <string>
#include <set>
using namespace std;

void test01(){
    pair<string,int> p1("Tom",18);
    cout<<p1.first<<","<<p1.second<<endl;
    pair<string,int> p2 = make_pair("Jerry",30);
    cout<<p2.first<<","<<p2.second<<endl;
}
int main() {
    test01();
    return 0;
}
```

总结：

两种方式都可以创建对组，记住一种即可



#### 3.8.8 set容器排序

学习目标：

- set容器默认排序规则为从小到大，掌握如何改变排序规则

主要技术点：

- 利用仿函数，可以改变排序规则

**示例一** set存放内置数据类型

```cpp
//
// Created by JSQ on 2021/7/10.
//
//set容器排序
#include <iostream>
#include <string>
#include <set>
using namespace std;
class MyCompare{
public:
   bool operator()(int v1,int v2){
       return v1>v2;
   }
};
void printSet(set<int> &s1) {
    for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << (*it) << " ";
    }
    cout << endl;
}

void test01(){
    set<int>s1;//默认从小到大排序
    s1.insert(10);
    s1.insert(20);
    s1.insert(40);
    s1.insert(30);
    printSet(s1);
    set<int,MyCompare>s2;//修改排序方式 从大到小
    s2.insert(10);
    s2.insert(20);
    s2.insert(40);
    s2.insert(30);
    for (set<int,MyCompare>::iterator it = s2.begin(); it != s2.end(); it++) {
        cout << (*it) << " ";
    }
    cout << endl;
}
int main() {
    test01();
    return 0;
}
```

总结：利用仿函数可以指定set容器的排序规则

**示例二** set存放自定义数据类型

```cpp
//
// Created by JSQ on 2021/7/10.
//
//set容器排序
#include <iostream>
#include <string>
#include <set>
using namespace std;
class Person{
public:
    Person(string nam,int a){
        this->age = a;
        this->name = nam;
    }
    string name;
    int age;
};
class MyCompare{
public:
    bool operator()(const Person &p1,const Person &p2){
        return p1.age>p2.age;
    }
};

void test01(){
    //自定义的数据类型 都会指定规则
    set<Person,MyCompare>s1;
    Person p1("刘备", 35 );
    Person p2("曹操", 45 );
    Person p3("孙权", 40 );
    Person p4("赵云", 25 );
    Person p5("张飞", 35 );
    Person p6("关羽", 35 );

    s1.insert(p1);
    s1.insert(p2);
    s1.insert(p3);
    s1.insert(p4);
    s1.insert(p5);
    s1.insert(p6);
    for(set<Person,MyCompare>::iterator it = s1.begin();it!=s1.end();it++){
        cout<<(*it).name<<","<<(*it).age<<endl;
    }
}
int main() {
    test01();
    return 0;
}
```

总结：

对于自定义数据类型，set必须指定排序规则才可以插入数据



### 3.9 map/ multimap容器

#### 3.9.1 map基本概念

**简介：**

- map中所有元素都是pair
- pair中第一个元素为key（键值），起到索引作用，第二个元素为value（实值）
- 所有元素都会根据元素的键值自动排序

**本质：**

- map/multimap属于**关联式容器**，底层结构是用二叉树实现。

**优点：**

- 可以根据key值快速找到value值

map和multimap**区别**：

- map不允许容器中有重复key值元素
- multimap允许容器中有重复key值元素

#### 3.9.2 map构造和赋值

**功能描述：**

- 对map容器进行构造和赋值操作

**函数原型：**

**构造：**

- `map<T1, T2> mp;` //map默认构造函数:
- `map(const map &mp);` //拷贝构造函数

**赋值：**

- `map& operator=(const map &mp);` //重载等号操作符

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <map>
using namespace std;
//map容器构造和赋值
void printMap(map<int,int>&m){
    for(map<int,int>::iterator it = m.begin();it!=m.end();it++){
        cout<<"key="<<(*it).first<<",value="<<(*it).second<<endl;
    }
    cout<<endl;
}
void test01(){
    map<int,int>m;
    m.insert(pair<int,int>(1,10));//匿名对组key=1 value=10
    m.insert(pair<int,int>(2,30));
    m.insert(pair<int,int>(5,15));
    m.insert(pair<int,int>(10,40));
    m.insert(pair<int,int>(3,70));
    m.insert(pair<int,int>(3,75));//不可以重复插入key值，插入后并没有发生修改
    printMap(m);

    //拷贝构造
    map<int,int>m2(m);
    printMap(m2);
    //赋值
    map<int,int>m3;
    m3 = m2;
    printMap(m3);
}
int main(){
    test01();
    return 0;
}
```

总结：map中所有元素都是成对出现，插入数据时候要使用对组



#### 3.9.3 map大小和交换

**功能描述：**

- 统计map容器大小以及交换map容器

函数原型：

- `size();` //返回容器中元素的数目
- `empty();` //判断容器是否为空
- `swap(st);` //交换两个集合容器

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <map>
using namespace std;
//map大小和交换
void printMap(map<int,int>&m){
    for(map<int,int>::iterator it = m.begin();it!=m.end();it++){
        cout<<"key="<<(*it).first<<",value="<<(*it).second<<endl;
    }
    cout<<endl;
}
void test01(){
    map<int,int>m;
    m.insert(pair<int,int>(1,10));//匿名对组key=1 value=10
    m.insert(pair<int,int>(2,30));
    m.insert(pair<int,int>(5,15));
    m.insert(pair<int,int>(10,40));
    m.insert(pair<int,int>(3,70));
    m.insert(pair<int,int>(3,75));//不可以重复插入key值，插入后并没有发生修改
    printMap(m);

    if(m.empty()){
        cout<<"Empty"<<endl;
    } else{
        cout<<"Size="<<m.size()<<endl;
    }
    map<int,int>m2;
    m2.insert(pair<int,int>(2,30));
    m2.insert(pair<int,int>(5,15));
    m2.insert(pair<int,int>(10,40));
    m2.insert(pair<int,int>(3,75));
    printMap(m2);
    m.swap(m2);
    printMap(m);
    printMap(m2);
}
int main(){
    test01();
    return 0;
}
```

总结：

- 统计大小 — size
- 判断是否为空 — empty
- 交换容器 — swap



#### 3.9.4 map插入和删除

**功能描述：**

- map容器进行插入数据和删除数据

**函数原型：**

- `insert(elem);` //在容器中插入元素。
- `clear();` //清除所有元素
- `erase(pos);` //删除pos迭代器所指的元素，返回下一个元素的迭代器。
- `erase(beg, end);` //删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
- `erase(key);` //删除容器中值为key的元素。

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <map>
using namespace std;
//map大小和交换
void printMap(map<int,int>&m){
    for(map<int,int>::iterator it = m.begin();it!=m.end();it++){
        cout<<"key="<<(*it).first<<",value="<<(*it).second<<endl;
    }
    cout<<endl;
}
void test01(){
    map<int,int>m;
    m.insert(pair<int,int>(1,10));//匿名对组key=1 value=10
    m.insert(pair<int,int>(2,30));
    m.insert(pair<int,int>(5,15));
    m.insert(pair<int,int>(10,40));
    m.insert(pair<int,int>(3,70));
    printMap(m);
    //第二种插入方式
    m.insert(make_pair(7,10));
    //第三种
    m.insert(map<int,int>::value_type(8,15));
    //第四种
    //不推荐-因为这种方式如果当前key在map中不存在会创建一个为0的key，value；有可能会产生错误的内容
    //可以利用key访问到value
    m[4] = 30;
    printMap(m);
    //删除
    m.erase(m.begin());
    printMap(m);
    m.erase(10);//按照key删除
    printMap(m);
    m.erase(++m.begin(),--m.end());//清空
    printMap(m);
    //清空
    m.clear();
    printMap(m);
}
int main(){
    test01();
    return 0;
}
```

总结：

- map插入方式很多，记住其一即可

- 插入 — insert
- 删除 — erase
- 清空 — clear



#### 3.9.5 map查找和统计

**功能描述：**

- 对map容器进行查找数据以及统计数据

**函数原型：**

- `find(key);` //查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
- `count(key);` //统计key的元素个数

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <string>
#include <map>
using namespace std;
//map查找和统计
void printMap(map<int,int>&m){
    for(map<int,int>::iterator it = m.begin();it!=m.end();it++){
        cout<<"key="<<(*it).first<<",value="<<(*it).second<<endl;
    }
    cout<<endl;
}
void test01(){
    map<int,int>m;
    m.insert(pair<int,int>(1,10));
    m.insert(pair<int,int>(2,30));
    m.insert(pair<int,int>(5,15));
    m.insert(pair<int,int>(6,40));
    m.insert(pair<int,int>(3,70));
    m.insert(make_pair(4,20));
    printMap(m);
    map<int,int>::iterator  pos = m.find(7);
    if (pos!=m.end()){
        cout<<"find key:"<<(*pos).first<<"value="<<(*pos).second<<endl;
    } else{
        cout<<"not found!"<<endl;
    }
    //map中 只有0，1
    //map中不允许插入重复key元素
    int num = m.count(3);
    cout<<"num="<<num<<endl;

    multimap<int,int>mm;
    mm.insert(make_pair(1,20));
    mm.insert(make_pair(1,30));
    mm.insert(make_pair(2,30));
    mm.insert(make_pair(2,40));
    for(multimap<int,int>::iterator it = mm.begin();it!=mm.end();it++){
        cout<<"key="<<(*it).first<<",value="<<(*it).second<<endl;
    }
    cout<<endl;

    multimap<int,int>::iterator  mpos = mm.find(1);
    if (mpos!=m.end()){
        cout<<"find key:"<<(*mpos).first<<",value="<<(*mpos).second<<endl;
    } else{
        cout<<"not found!"<<endl;
    }
    //map中 只有0，1
    //map中不允许插入重复key元素
    int mnum = mm.count(2);
    cout<<"num="<<mnum<<endl;
}
int main(){
    test01();
    return 0;
}
```

总结：

- 查找 — find （返回的是迭代器）
- 统计 — count （对于map，结果为0或者1）



#### 3.9.6 map容器排序

**学习目标：**

- map容器默认排序规则为 按照key值进行 从小到大排序，掌握如何改变排序规则

**主要技术点:**

- 利用仿函数，可以改变排序规则

**示例：**

```cpp
//
// Created by JSQ on 2021/7/10.
//
#include <iostream>
#include <map>
#include <string>
using namespace std;
//map 排序
class Person{
public:
    Person(string n,int a){
        this->name = n;
        this->age = a;
    }
    string name;
    int age;
};
class MyCompare{
public:
    bool operator()(int v1,int v2){
        return v1>v2;
    }
    bool operator()(const Person &p1,const Person &p2){
        return p1.age>p2.age;
    }
};

void test01(){
    map<int,int,MyCompare>m;
    m.insert(make_pair(1,10));
    m.insert(make_pair(2,20));
    m.insert(make_pair(3,30));
    m.insert(make_pair(4,40));
    for(map<int,int,MyCompare>::iterator it = m.begin();it!=m.end();it++){
        cout<<(*it).first<<","<<(*it).second<<endl;
    }
    cout<<endl;

    map<int,Person,MyCompare>mp;
    Person p1("刘备", 35 );
    Person p2("曹操", 45 );
    Person p3("孙权", 40 );
    Person p4("赵云", 25 );
    Person p5("张飞", 35 );
    Person p6("关羽", 35 );
    mp.insert(make_pair(1,p1));
    mp.insert(make_pair(2,p2));
    mp.insert(make_pair(3,p3));
    mp.insert(make_pair(4,p4));
    mp.insert(make_pair(5,p5));
    for(map<int,Person,MyCompare>::iterator it = mp.begin();it!=mp.end();it++){
        cout<<(*it).first<<","<<(*it).second.name<<":"<<(*it).second.age<<endl;
    }
    cout<<endl;

    map<Person,int,MyCompare>mp2;
    mp2.insert(make_pair(p1,1));
    mp2.insert(make_pair(p2,2));
    mp2.insert(make_pair(p3,3));
    mp2.insert(make_pair(p4,4));
    mp2.insert(make_pair(p5,5));
    for(map<Person,int,MyCompare>::iterator it = mp2.begin();it!=mp2.end();it++){
        cout<<(*it).second<<","<<(*it).first.name<<":"<<(*it).first.age<<endl;
    }
    cout<<endl;

}
int main(){
    test01();
    return 0;
}
```

总结：

- 利用仿函数可以指定map容器的排序规则
- 对于自定义数据类型，map必须要指定排序规则,同set容器



### 3.10 案例-员工分组

#### 3.10.1 案例描述

- 公司今天招聘了10个员工（ABCDEFGHIJ），10名员工进入公司之后，需要指派员工在那个部门工作
- 员工信息有: 姓名 工资组成；部门分为：策划、美术、研发
- 随机给10名员工分配部门和工资
- 通过multimap进行信息的插入 key(部门编号) value(员工)
- 分部门显示员工信息

#### 3.10.2 实现步骤

1. 创建10名员工，放到vector中
2. 遍历vector容器，取出每个员工，进行随机分组
3. 分组后，将员工部门编号作为key，具体员工作为value，放入到multimap容器中
4. 分部门显示员工信息

**案例代码：**

```cpp
#include <iostream>
#include <string>
using namespace std;
#include <vector>
#include <map>
#include <ctime>
/*
 #### 3.10.1 案例描述

- 公司今天招聘了10个员工（ABCDEFGHIJ），10名员工进入公司之后，需要指派员工在那个部门工作
- 员工信息有: 姓名 工资组成；部门分为：策划、美术、研发
- 随机给10名员工分配部门和工资
- 通过multimap进行信息的插入 key(部门编号) value(员工)
- 分部门显示员工信息

#### 3.10.2 实现步骤

1. 创建10名员工，放到vector中
2. 遍历vector容器，取出每个员工，进行随机分组
3. 分组后，将员工部门编号作为key，具体员工作为value，放入到multimap容器中
4. 分部门显示员工信息
 */
#define CEHUA 0
#define MEISHU 1
#define YANFA 2
class Worker{
public:
    string name;
    int salary;
};
void createWorker(vector<Worker>&vworker){
    string nameSeed = "ABCDEFGHIJ";
    for (int i = 0; i < 10; ++i) {
        Worker worker;
        worker.name = "员工";
        worker.name += nameSeed[i];
        worker.salary = rand()%10001 + 10000;
        vworker.push_back(worker);
    }
}
void showWorker(vector<Worker>&vworker){
    for(vector<Worker>::iterator it = vworker.begin();it!=vworker.end();it++){
        cout<<it->name<<","<<it->salary<<endl;
    }
}
void setGroup(vector<Worker>&vworker,multimap<int,Worker>&mworker){
    for(vector<Worker>::iterator it = vworker.begin();it!=vworker.end();it++){
        //产生随机部门编号
        int depId = rand()%3;//0,1,2
        //将员工插入分组
        mworker.insert(make_pair(depId,*it));
    }
}
void showWorkerByGroup(multimap<int,Worker>&mworker){
    cout<<"策划部门："<<endl;
    multimap<int,Worker>::iterator  pos = mworker.find(CEHUA);
    int count = mworker.count(CEHUA);
    int index = 0;
    for(;pos != mworker.end()&&index<count;pos++,index++){
        cout<<pos->second.name<<","<<pos->second.salary<<endl;
    }
    cout<<"美术部门："<<endl;
    pos = mworker.find(MEISHU);
    count = mworker.count(MEISHU);
    index = 0;
    for(;pos != mworker.end()&&index<count;pos++,index++){
        cout<<pos->second.name<<","<<pos->second.salary<<endl;
    }
    cout<<"研发部门："<<endl;
    pos = mworker.find(YANFA);
    count = mworker.count(YANFA);
    index = 0;
    for(;pos != mworker.end()&&index<count;pos++,index++){
        cout<<pos->second.name<<","<<pos->second.salary<<endl;
    }

}
int main() {
    srand((unsigned int) time(NULL));
    //创建员工
    vector<Worker>vworker;
    createWorker(vworker);
//    showWorker(vworker);
    //员工分组
    multimap<int,Worker>mworker;
    setGroup(vworker,mworker);
    //分组显示员工
    showWorkerByGroup(mworker);
    return 0;
}
```

总结：

- 当数据以键值对形式存在，可以考虑用map 或 multimap



## 4 STL- 函数对象

### 4.1 函数对象

#### 4.1.1 函数对象概念

**概念：**

- 重载**函数调用操作符**的类，其对象常称为**函数对象**
- **函数对象**使用重载的()时，行为类似函数调用，也叫**仿函数**

**本质：**

函数对象(仿函数)是一个**类**，不是一个函数

#### 4.1.2 函数对象使用

**特点：**

- 函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
- 函数对象超出普通函数的概念，函数对象可以有自己的状态
- 函数对象可以作为参数传递

**示例:**

```cpp
#include <iostream>
#include <string>
using namespace std;
//函数对象的使用-仿函数
class MyAdd{
public:
    MyAdd(){
        count = 0;
    }
    int operator()(int v1,int v2){
        count++;
        return v1+v2;
    }
    int count;
};
//1. 函数对象在使用时，可以像普通函数那样调用，可以有参数，返回值
void test01(){
    MyAdd myadd;
    cout<<myadd(10,10)<<endl;
    cout<<endl;
}
// 2.函数对象超出普通函数的概念，可以有自己的状态
void test02(){
    MyAdd myadd;
    cout<<myadd(10,10)<<endl;
    cout<<myadd(10,10)<<endl;
    cout<<myadd(10,10)<<endl;
    cout<<myadd(10,10)<<endl;
    cout<<myadd.count<<endl;//调用的次数
    cout<<endl;
}
// 3.函数对象可以作为参数传递
void doWork(MyAdd &ma,int n1,int n2){
    cout<<ma(n1,n2)<<endl;
}
void test03(){
    MyAdd myadd;
    doWork(myadd,29,39);
}
int main() {
    test01();
    test02();
    test03();
    return 0;
}
```

总结：

- 仿函数写法非常灵活，可以作为参数进行传递。



### 4.2 谓词

#### 4.2.1 谓词概念

**概念：**

- 返回bool类型的仿函数称为**谓词**
- 如果operator()接受一个参数，那么叫做一元谓词
- 如果operator()接受两个参数，那么叫做二元谓词

#### 4.2.2 一元谓词

**示例：**

```cpp
#include <iostream>
using namespace std;
#include <vector>
#include <algorithm>
//一元谓词
//仿函数 返回值类型是bool数据类型，成为谓词
class FindFive{
public:
    bool operator()(int val){
        return val>5;
    }
};
void test01(){
    vector<int>v;
    for(int i = 0;i<10;i++){
        v.push_back(i);
    }
    //查找容器中 有没有大于5的数字
    vector<int>::iterator  it = find_if(v.begin(), v.end(),FindFive());//FindFive()匿名函数对象
    if(it==v.end()){
        cout<<"not Found!"<<endl;
    } else{
        cout<<"Found!"<<*it<<endl;
    }
}
int main() {
    test01();
    return 0;
}
```

总结：参数只有一个的谓词，称为一元谓词



#### 4.2.3 二元谓词

**示例：**

```cpp
##include <iostream>
using namespace std;
#include <vector>
#include <algorithm>
//二元谓词-两个参数
//仿函数 返回值类型是bool数据类型，成为谓词
class MyCompare{
public:
    bool operator()(int v1,int v2){
        return v1>v2;
    }
};
void test01(){
    vector<int>v;
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(3);

    sort(v.begin(), v.end(),MyCompare());//匿名函数对象-降序排列
    for(vector<int>::iterator it = v.begin();it!=v.end();it++){
        cout << *it<<" ";
    }
    cout<<endl;
}
int main() {
    test01();
    return 0;
}
```

总结：参数只有两个的谓词，称为二元谓词



### 4.3 内建函数对象

#### 4.3.1 内建函数对象意义

**概念：**

- STL内建了一些函数对象

**分类:**

- 算术仿函数
- 关系仿函数
- 逻辑仿函数

**用法：**

- 这些仿函数所产生的对象，用法和一般函数完全相同
- 使用内建函数对象，需要引入头文件 `#include<functional>`

#### 4.3.2 算术仿函数

**功能描述：**

- 实现四则运算
- 其中negate是一元运算，其他都是二元运算

**仿函数原型：**

- `template<class T> T plus<T>` //加法仿函数
- `template<class T> T minus<T>` //减法仿函数
- `template<class T> T multiplies<T>` //乘法仿函数
- `template<class T> T divides<T>` //除法仿函数
- `template<class T> T modulus<T>` //取模仿函数
- `template<class T> T negate<T>` //取反仿函数

**示例：**

```cpp
#include <iostream>
#include <string>
#include <functional>
using namespace std;
//内建函数对象 - 算术仿函数

void test01(){
    //negate 一元仿函数 取反
    negate<int>n;
    cout<<n(50)<<endl;
    //plus 二元仿函数 加法
    plus<int>p;
    cout<<p(10,30)<<endl;
}


using namespace std;
int main() {
    test01();
    return 0;
}
```

总结：使用内建函数对象时，需要引入头文件 `#include <functional>`



#### 4.3.3 关系仿函数

**功能描述：**

- 实现关系对比

**仿函数原型：**

- `template<class T> bool equal_to<T>` //等于
- `template<class T> bool not_equal_to<T>` //不等于
- `template<class T> bool greater<T>` //大于
- `template<class T> bool greater_equal<T>` //大于等于
- `template<class T> bool less<T>` //小于
- `template<class T> bool less_equal<T>` //小于等于

**示例：**

```cpp
//
// Created by JSQ on 2021/7/11.
//
#include <vector>
#include <iostream>
#include <string>
#include <functional>
#include <algorithm>
using namespace std;
//内建函数对象 - 关系仿函数
class MyCMP{
public:
    bool operator()(int v1,int v2){
        return v1>v2;
    }
};
void test01(){
    vector<int>v;
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(3);
    //降序
//    sort(v.begin(), v.end(),MyCMP());
    //内建函数提供
    sort(v.begin(), v.end(),greater<int>());
    for(vector<int>::iterator it = v.begin();it!=v.end();it++){
        cout << *it<<" ";
    }
    cout<<endl;

}


using namespace std;
int main() {
    test01();
    return 0;
}
```

总结：关系仿函数中最常用的就是greater<>大于



#### 4.3.4 逻辑仿函数

**功能描述：**

- 实现逻辑运算

**函数原型：**

- `template<class T> bool logical_and<T>` //逻辑与
- `template<class T> bool logical_or<T>` //逻辑或
- `template<class T> bool logical_not<T>` //逻辑非

**示例：**

```cpp
//
// Created by JSQ on 2021/7/11.
//
#include <vector>
#include <iostream>
#include <string>
#include <functional>
#include <algorithm>
using namespace std;
//内建函数对象 - 逻辑仿函数
void test01(){
    vector<bool>v;
    v.push_back(1);
    v.push_back(0);
    v.push_back(1);
    v.push_back(0);
    v.push_back(1);

    for(vector<bool>::iterator it = v.begin();it!=v.end();it++){
        cout << *it<<" ";
    }
    cout<<endl;
    vector<bool>v2;
    //将v2扩充和v一样大小--必须的，否则无法成功搬移
    v2.resize(v.size());
    //对v进行逻辑非并将内容搬移到v2中
    transform(v.begin(), v.end(),v2.begin(),logical_not<bool>());
    for(vector<bool>::iterator it = v2.begin();it!=v2.end();it++){
        cout << *it<<" ";
    }
    cout<<endl;

}


using namespace std;
int main() {
    test01();
    return 0;
}
```

总结：逻辑仿函数实际应用较少，了解即可



## 5 STL- 常用算法

**概述**:

- 算法主要是由头文件`<algorithm>` `<functional>` `<numeric>`组成。
- `<algorithm>`是所有STL头文件中最大的一个，范围涉及到比较、 交换、查找、遍历操作、复制、修改等等
- `<numeric>`体积很小，只包括几个在序列上面进行简单数学运算的模板函数
- `<functional>`定义了一些模板类,用以声明函数对象。

### 5.1 常用遍历算法

**学习目标：**

- 掌握常用的遍历算法

**算法简介：**

- `for_each` //遍历容器
- `transform` //搬运容器到另一个容器中

#### 5.1.1 for_each

**功能描述：**

- 实现遍历容器

**函数原型：**

- `for_each(iterator beg, iterator end, _func);`

  // 遍历算法 遍历容器元素

  // beg 开始迭代器

  // end 结束迭代器

  // _func 函数或者函数对象

**示例：**

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
//常用遍历算法 for_each
using namespace std;
//普通函数
void print01(int val){
    cout<<val<<" ";
}
//仿函数
class MyPrint{
public:
    void operator()(int val){
        cout<<val<<" ";
    }
};
void test01(){
    vector<int>v;
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(3);
    for_each(v.begin(),v.end(), print01);
    cout<<endl;
    for_each(v.begin(),v.end(), MyPrint());
}
int main() {
    test01();
    return 0;
}
```

总结：**for_each在实际开发中是最常用遍历算法，需要熟练掌握**



#### 5.1.2 transform

**功能描述：**

- 搬运容器到另一个容器中

**函数原型：**

- `transform(iterator beg1, iterator end1, iterator beg2, _func);`

//beg1 源容器开始迭代器

//end1 源容器结束迭代器

//beg2 目标容器开始迭代器

//_func 函数或者函数对象

**示例：**

```cpp
//
// Created by JSQ on 2021/7/11.
//
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
//常用遍历算法 transform
using namespace std;
class Transform{
public:
    int operator()(int val){
        return val+100;
    }
};
class MyPrint{
public:
    void operator()(int val){
        cout<<val<<" ";
    }
};
void test01(){
    vector<int>v;
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(3);
    vector<int>v2;//目标容器
    //必须要提前开辟空间
    v2.resize(v.size());
    transform(v.begin(),v.end(),v2.begin(),Transform());
    for_each(v2.begin(),v2.end(), MyPrint());
}
int main() {
    test01();
    return 0;
}

```

**总结：** 搬运的目标容器必须要提前开辟空间，否则无法正常搬运



### 5.2 常用查找算法

学习目标：

- 掌握常用的查找算法

**算法简介：**

- `find` //查找元素
- `find_if` //按条件查找元素
- `adjacent_find` //查找相邻重复元素
- `binary_search` //二分查找法
- `count` //统计元素个数
- `count_if` //按条件统计元素个数

#### 5.2.1 find

**功能描述：**

- 查找指定元素，找到返回指定元素的迭代器，找不到返回结束迭代器end()

**函数原型：**

- `find(iterator beg, iterator end, value);`

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg 开始迭代器

  // end 结束迭代器

  // value 查找的元素

**示例：**

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
using namespace std;
//查找元素find
//查找内置数据
void test01(){
    vector<int>v;
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(3);
    vector<int>::iterator pos = find(v.begin(),v.end(),5);
    if(pos==v.end()){
        cout<<"not Found"<<endl;
    }else{
        cout<<"Found!"<<"->"<<*pos<<endl;
    }
}
class Person{
public:
    Person(string n,int a){
        this->name =n;
        this->age = a;
    }
    //重载 == 让底层find知道如何对比Person数据类型
    bool operator==(const Person &p){
        if(this->name == p.name && this->age == p.age){
            return true;
        }else{
            return false;
        }
    }
    string name;
    int age;
};
//查找自定义数据类型
void test02(){
    vector<Person>v;
    Person p1("刘备", 35 );
    Person p2("曹操", 45 );
    Person p3("孙权", 40 );
    Person p4("赵云", 25 );
    Person p5("张飞", 35 );
    Person p6("关羽", 35 );

    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    v.push_back(p4);
    v.push_back(p5);
    v.push_back(p6);
    vector<Person>::iterator pos = find(v.begin(),v.end(),p6);
    if(pos==v.end()){
        cout<<"not Found"<<endl;
    }else{
        cout<<"Found!"<<"->"<<(*pos).name<<","<<(*pos).age<<endl;
    }
}
int main() {
    test02();
    return 0;
}
```

总结： 利用find可以在容器中找指定的元素，返回值是**迭代器**



#### 5.2.2 find_if

**功能描述：**

- 按条件查找元素

**函数原型：**

- `find_if(iterator beg, iterator end, _Pred);`

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg 开始迭代器

  // end 结束迭代器

  // _Pred 函数或者谓词（返回bool类型的仿函数）

**示例：**

```cpp
//
// Created by JSQ on 2021/7/11.
//
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
using namespace std;
//查找元素find_if
//查找内置数据
class GreaterFour{
public:
    bool operator()(int val){
        return val>4;
    }
};
void test01(){
    vector<int>v;
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(3);
    vector<int>::iterator pos = find_if(v.begin(),v.end(),GreaterFour());
    if(pos==v.end()){
        cout<<"not Found"<<endl;
    }else{
        cout<<"Found!"<<"->"<<*pos<<endl;
    }
}

class Person{
public:
    Person(string n,int a){
        this->name =n;
        this->age = a;
    }
    //重载 == 让底层find知道如何对比Person数据类型
    bool operator==(const Person &p){
        if(this->name == p.name && this->age == p.age){
            return true;
        }else{
            return false;
        }
    }
    string name;
    int age;
};
class Less30{
public:
    bool operator()( Person &p){
        return p.age<30;
    }
};
//查找自定义数据类型
void test02(){
    vector<Person>v;
    Person p1("刘备", 35 );
    Person p2("曹操", 45 );
    Person p3("孙权", 40 );
    Person p4("赵云", 25 );
    Person p5("张飞", 35 );
    Person p6("关羽", 35 );

    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    v.push_back(p4);
    v.push_back(p5);
    v.push_back(p6);
    vector<Person>::iterator pos = find_if(v.begin(),v.end(),Less30());
    if(pos==v.end()){
        cout<<"not Found"<<endl;
    }else{
        cout<<"Found!"<<"->"<<(*pos).name<<","<<(*pos).age<<endl;
    }
}
int main() {
    test02();
    return 0;
}
```

总结：find_if按条件查找使查找更加灵活，提供的仿函数可以改变不同的策略



#### 5.2.3 adjacent_find

**功能描述：**

- 查找相邻重复元素

**函数原型：**

- `adjacent_find(iterator beg, iterator end);`

  // 查找相邻重复元素,返回相邻元素的第一个位置的迭代器

  // beg 开始迭代器

  // end 结束迭代器

  

**示例：**

```cpp
//
// Created by JSQ on 2021/7/14.
//
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
using namespace std;
//查找元素adjacent_find-相邻重复数据
//查找内置数据
void test01(){
    vector<int>v;
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(3);
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(3);
    v.push_back(3);
    vector<int>::iterator pos = adjacent_find(v.begin(),v.end());
    if(pos==v.end()){
        cout<<"not Found"<<endl;
    }else{
        cout<<"Found!"<<"->"<<*pos<<endl;
    }
}
int main() {
    test01();
    return 0;
}
```

总结：面试题中如果出现查找相邻重复元素，记得用STL中的adjacent_find算法



#### 5.2.4 binary_search

**功能描述：**

- 查找指定元素是否存在

**函数原型：**

- `bool binary_search(iterator beg, iterator end, value);`

  // 查找指定的元素，查到 返回true 否则false

  // 注意: 在**无序序列中不可用**

  // beg 开始迭代器

  // end 结束迭代器

  // value 查找的元素

**示例：**

```cpp
//
// Created by JSQ on 2021/7/14.
//
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
using namespace std;
//查找元素binary_search-指定元素是否存在---无序序列中不可用
//查找内置数据
void test01(){
    vector<int>v;
    for (int i = 0; i < 10; ++i) {
        v.push_back(i);
    }
    v.push_back(2);//加入后变成无序序列，结果未知！！！！
    bool ret = binary_search(v.begin(),v.end(),9);
    if(!ret){
        cout<<"not Found"<<endl;
    }else{
        cout<<"Found!"<<endl;
    }
}
int main() {
    test01();
    return 0;
}
```

**总结：**二分查找法查找效率很高，值得注意的是查找的容器中元素必须的有序序列



#### 5.2.5 count

**功能描述：**

- 统计元素个数

**函数原型：**

- `count(iterator beg, iterator end, value);`

  // 统计元素出现次数

  // beg 开始迭代器

  // end 结束迭代器

  // value 统计的元素

**示例：**

```cpp
//
// Created by JSQ on 2021/7/14.
//
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
using namespace std;
//查找元素find_if
//查找内置数据
class GreaterFour{
public:
    bool operator()(int val){
        return val>4;
    }
};
void test01(){
    vector<int>v;
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(3);
    v.push_back(4);
    v.push_back(4);
    int num = count(v.begin(),v.end(),4);
    cout<<num<<endl;

}

class Person{
public:
    Person(string n,int a){
        this->name =n;
        this->age = a;
    }
    //重载 == 让底层find知道如何对比Person数据类型
    bool operator==(const Person &p){
        if(this->age == p.age){
            return true;
        }else{
            return false;
        }
    }
    string name;
    int age;
};
class Less30{
public:
    bool operator()( Person &p){
        return p.age<30;
    }
};
//查找自定义数据类型
void test02(){
    vector<Person>v;
    Person p1("刘备", 35 );
    Person p2("曹操", 45 );
    Person p3("孙权", 40 );
    Person p4("赵云", 25 );
    Person p5("张飞", 35 );
    Person p6("关羽", 35 );
    Person p7("诸葛亮", 35 );
    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    v.push_back(p4);
    v.push_back(p5);
    v.push_back(p6);
    int num = count(v.begin(),v.end(),p7);
    cout<<"35岁："<<num<<endl;
}
int main() {
    test02();
    return 0;
}
```

**总结：** 统计自定义数据类型时候，需要配合重载 `operator==`



#### 5.2.6 count_if

**功能描述：**

- 按条件统计元素个数

**函数原型：**

- `count_if(iterator beg, iterator end, _Pred);`

  // 按条件统计元素出现次数

  // beg 开始迭代器

  // end 结束迭代器

  // _Pred 谓词

  

**示例：**

```cpp
//
// Created by JSQ on 2021/7/14.
//
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
using namespace std;
//统计数据count_if
//查找内置数据
class GreaterFour{
public:
    bool operator()(int val){
        return val>=4;
    }
};
void test01(){
    vector<int>v;
    v.push_back(1);
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(3);
    v.push_back(4);
    v.push_back(4);
    int num = count_if(v.begin(),v.end(),GreaterFour());
    cout<<num<<endl;

}

class Person{
public:
    Person(string n,int a){
        this->name =n;
        this->age = a;
    }
    //重载 == 让底层find知道如何对比Person数据类型
    bool operator==(const Person &p){
        if(this->age == p.age){
            return true;
        }else{
            return false;
        }
    }
    string name;
    int age;
};
class Greater30{
public:
    bool operator()(const Person &p){
        return p.age>30;
    }
};
//查找自定义数据类型
void test02(){
    vector<Person>v;
    Person p1("刘备", 35 );
    Person p2("曹操", 45 );
    Person p3("孙权", 40 );
    Person p4("赵云", 25 );
    Person p5("张飞", 35 );
    Person p6("关羽", 35 );
    Person p7("诸葛亮", 35 );
    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    v.push_back(p4);
    v.push_back(p5);
    v.push_back(p6);
    int num = count_if(v.begin(),v.end(),Greater30());
    cout<<"大于35岁："<<num<<endl;
}
int main() {
    test02();
    return 0;
}
```

**总结：**按值统计用count，按条件统计用count_if



### 5.3 常用排序算法

**学习目标：**

- 掌握常用的排序算法

**算法简介：**

- `sort` //对容器内元素进行排序
- `random_shuffle` //洗牌 指定范围内的元素随机调整次序
- `merge` // 容器元素合并，并存储到另一容器中
- `reverse` // 反转指定范围的元素

#### 5.3.1 sort

**功能描述：**

- 对容器内元素进行排序

**函数原型：**

- `sort(iterator beg, iterator end, _Pred);`

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg 开始迭代器

  // end 结束迭代器

  // _Pred 谓词

**示例：**

```cpp
#include <algorithm>
#include <vector>

void myPrint(int val)
{
	cout << val << " ";
}

void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(30);
	v.push_back(50);
	v.push_back(20);
	v.push_back(40);

	//sort默认从小到大排序
	sort(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint);
	cout << endl;

	//从大到小排序
	sort(v.begin(), v.end(), greater<int>());
	for_each(v.begin(), v.end(), myPrint);
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
1234567891011121314151617181920212223242526272829303132333435
```

**总结**：sort属于开发中最常用的算法之一，需熟练掌握

#### 5.3.2 random_shuffle

**功能描述：**

- 洗牌 指定范围内的元素随机调整次序

**函数原型：**

- `random_shuffle(iterator beg, iterator end);`

  // 指定范围内的元素随机调整次序

  // beg 开始迭代器

  // end 结束迭代器

  

**示例：**

```cpp
#include <algorithm>
#include <vector>
#include <ctime>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	srand((unsigned int)time(NULL));
	vector<int> v;
	for(int i = 0 ; i < 10;i++)
	{
		v.push_back(i);
	}
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	//打乱顺序
	random_shuffle(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
1234567891011121314151617181920212223242526272829303132333435363738
```

**总结**：random_shuffle洗牌算法比较实用，使用时记得加随机数种子

#### 5.3.3 merge

**功能描述：**

- 两个容器元素合并，并存储到另一容器中

**函数原型：**

- `merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

  // 容器元素合并，并存储到另一容器中

  // 注意: 两个容器必须是**有序的**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器

  

**示例：**

```cpp
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10 ; i++) 
    {
		v1.push_back(i);
		v2.push_back(i + 1);
	}

	vector<int> vtarget;
	//目标容器需要提前开辟空间
	vtarget.resize(v1.size() + v2.size());
	//合并  需要两个有序序列
	merge(v1.begin(), v1.end(), v2.begin(), v2.end(), vtarget.begin());
	for_each(vtarget.begin(), vtarget.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
123456789101112131415161718192021222324252627282930313233343536373839
```

**总结**：merge合并的两个容器必须的有序序列

#### 5.3.4 reverse

**功能描述：**

- 将容器内元素进行反转

**函数原型：**

- `reverse(iterator beg, iterator end);`

  // 反转指定范围的元素

  // beg 开始迭代器

  // end 结束迭代器

  

**示例：**

```cpp
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v;
	v.push_back(10);
	v.push_back(30);
	v.push_back(50);
	v.push_back(20);
	v.push_back(40);

	cout << "反转前： " << endl;
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	cout << "反转后： " << endl;

	reverse(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
12345678910111213141516171819202122232425262728293031323334353637383940
```

**总结**：reverse反转区间内元素，面试题可能涉及到

### 5.4 常用拷贝和替换算法

**学习目标：**

- 掌握常用的拷贝和替换算法

**算法简介：**

- `copy` // 容器内指定范围的元素拷贝到另一容器中
- `replace` // 将容器内指定范围的旧元素修改为新元素
- `replace_if` // 容器内指定范围满足条件的元素替换为新元素
- `swap` // 互换两个容器的元素

#### 5.4.1 copy

**功能描述：**

- 容器内指定范围的元素拷贝到另一容器中

**函数原型：**

- `copy(iterator beg, iterator end, iterator dest);`

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg 开始迭代器

  // end 结束迭代器

  // dest 目标起始迭代器

**示例：**

```cpp
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i + 1);
	}
	vector<int> v2;
	v2.resize(v1.size());
	copy(v1.begin(), v1.end(), v2.begin());

	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
12345678910111213141516171819202122232425262728293031323334
```

**总结：**利用copy算法在拷贝时，目标容器记得提前开辟空间

#### 5.4.2 replace

**功能描述：**

- 将容器内指定范围的旧元素修改为新元素

**函数原型：**

- `replace(iterator beg, iterator end, oldvalue, newvalue);`

  // 将区间内旧元素 替换成 新元素

  // beg 开始迭代器

  // end 结束迭代器

  // oldvalue 旧元素

  // newvalue 新元素

**示例：**

```cpp
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v;
	v.push_back(20);
	v.push_back(30);
	v.push_back(20);
	v.push_back(40);
	v.push_back(50);
	v.push_back(10);
	v.push_back(20);

	cout << "替换前：" << endl;
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	//将容器中的20 替换成 2000
	cout << "替换后：" << endl;
	replace(v.begin(), v.end(), 20,2000);
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
123456789101112131415161718192021222324252627282930313233343536373839404142
```

**总结**：replace会替换区间内满足条件的元素

#### 5.4.3 replace_if

**功能描述:**

- 将区间内满足条件的元素，替换成指定元素

**函数原型：**

- `replace_if(iterator beg, iterator end, _pred, newvalue);`

  // 按条件替换元素，满足条件的替换成指定元素

  // beg 开始迭代器

  // end 结束迭代器

  // _pred 谓词

  // newvalue 替换的新元素

**示例：**

```cpp
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

class ReplaceGreater30
{
public:
	bool operator()(int val)
	{
		return val >= 30;
	}

};

void test01()
{
	vector<int> v;
	v.push_back(20);
	v.push_back(30);
	v.push_back(20);
	v.push_back(40);
	v.push_back(50);
	v.push_back(10);
	v.push_back(20);

	cout << "替换前：" << endl;
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	//将容器中大于等于的30 替换成 3000
	cout << "替换后：" << endl;
	replace_if(v.begin(), v.end(), ReplaceGreater30(), 3000);
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

总结：**replace_if按条件查找，可以利用仿函数灵活筛选满足的条件**

#### 5.4.4 swap

**功能描述：**

- 互换两个容器的元素

**函数原型：**

- `swap(container c1, container c2);`

  // 互换两个容器的元素

  // c1容器1

  // c2容器2

  

**示例：**

```cpp
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i+100);
	}

	cout << "交换前： " << endl;
	for_each(v1.begin(), v1.end(), myPrint());
	cout << endl;
	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;

	cout << "交换后： " << endl;
	swap(v1, v2);
	for_each(v1.begin(), v1.end(), myPrint());
	cout << endl;
	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

总结：**swap交换容器时，注意交换的容器要同种类型**

### 5.5 常用算术生成算法

**学习目标：**

- 掌握常用的算术生成算法

**注意：**

- 算术生成算法属于小型算法，使用时包含的头文件为 `#include <numeric>`

**算法简介：**

- `accumulate` // 计算容器元素累计总和

- `fill` // 向容器中添加元素

  

#### 5.5.1 accumulate

**功能描述：**

- 计算区间内 容器元素累计总和

**函数原型：**

- `accumulate(iterator beg, iterator end, value);`

  // 计算容器元素累计总和

  // beg 开始迭代器

  // end 结束迭代器

  // value 起始值

**示例：**

```cpp
#include <numeric>
#include <vector>
void test01()
{
	vector<int> v;
	for (int i = 0; i <= 100; i++) {
		v.push_back(i);
	}

	int total = accumulate(v.begin(), v.end(), 0);

	cout << "total = " << total << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
12345678910111213141516171819202122
```

**总结：**accumulate使用时头文件注意是 numeric，这个算法很实用

#### 5.5.2 fill

**功能描述：**

- 向容器中填充指定的元素

**函数原型：**

- `fill(iterator beg, iterator end, value);`

  // 向容器中填充元素

  // beg 开始迭代器

  // end 结束迭代器

  // value 填充的值

**示例：**

```cpp
#include <numeric>
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{

	vector<int> v;
	v.resize(10);
	//填充
	fill(v.begin(), v.end(), 100);

	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
123456789101112131415161718192021222324252627282930313233
```

**总结**：利用fill可以将容器区间内元素填充为 指定的值

### 5.6 常用集合算法

**学习目标：**

- 掌握常用的集合算法

**算法简介：**

- `set_intersection` // 求两个容器的交集

- `set_union` // 求两个容器的并集

- `set_difference` // 求两个容器的差集

  

#### 5.6.1 set_intersection

**功能描述：**

- 求两个容器的交集

**函数原型：**

- `set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

  // 求两个集合的交集

  // **注意:两个集合必须是有序序列**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器

**示例：**

```cpp
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++)
    {
		v1.push_back(i);
		v2.push_back(i+5);
	}

	vector<int> vTarget;
	//取两个里面较小的值给目标容器开辟空间
	vTarget.resize(min(v1.size(), v2.size()));

	//返回目标容器的最后一个元素的迭代器地址
	vector<int>::iterator itEnd = 
        set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());

	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
123456789101112131415161718192021222324252627282930313233343536373839404142
```

**总结：**

- 求交集的两个集合必须的有序序列
- 目标容器开辟空间需要从**两个容器中取小值**
- set_intersection返回值既是交集中最后一个元素的位置

#### 5.6.2 set_union

**功能描述：**

- 求两个集合的并集

**函数原型：**

- `set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

  // 求两个集合的并集

  // **注意:两个集合必须是有序序列**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器

  

**示例：**

```cpp
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i+5);
	}

	vector<int> vTarget;
	//取两个容器的和给目标容器开辟空间
	vTarget.resize(v1.size() + v2.size());

	//返回目标容器的最后一个元素的迭代器地址
	vector<int>::iterator itEnd = 
        set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());

	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
1234567891011121314151617181920212223242526272829303132333435363738394041
```

**总结：**

- 求并集的两个集合必须的有序序列
- 目标容器开辟空间需要**两个容器相加**
- set_union返回值既是并集中最后一个元素的位置

#### 5.6.3 set_difference

**功能描述：**

- 求两个集合的差集

**函数原型：**

- `set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

  // 求两个集合的差集

  // **注意:两个集合必须是有序序列**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器

  

**示例：**

```cpp
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i+5);
	}

	vector<int> vTarget;
	//取两个里面较大的值给目标容器开辟空间
	vTarget.resize( max(v1.size() , v2.size()));

	//返回目标容器的最后一个元素的迭代器地址
	cout << "v1与v2的差集为： " << endl;
	vector<int>::iterator itEnd = 
        set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;


	cout << "v2与v1的差集为： " << endl;
	itEnd = set_difference(v2.begin(), v2.end(), v1.begin(), v1.end(), vTarget.begin());
	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

**总结：**

- 求差集的两个集合必须的有序序列
- 目标容器开辟空间需要从**两个容器取较大值**

