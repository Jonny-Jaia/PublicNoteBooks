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

**迭代器**：容器和算法之间粘合剂

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
#include <vector>
#include <algorithm>

void MyPrint(int val)
{
	cout << val << endl;
}

void test01() {

	//创建vector容器对象，并且通过模板参数指定容器中存放的数据的类型
	vector<int> v;
	//向容器中放数据
	v.push_back(10);
	v.push_back(20);
	v.push_back(30);
	v.push_back(40);

	//每一个容器都有自己的迭代器，迭代器是用来遍历容器中的元素
	//v.begin()返回迭代器，这个迭代器指向容器中第一个数据
	//v.end()返回迭代器，这个迭代器指向容器元素的最后一个元素的下一个位置
	//vector<int>::iterator 拿到vector<int>这种容器的迭代器类型

	vector<int>::iterator pBegin = v.begin();
	vector<int>::iterator pEnd = v.end();

	//第一种遍历方式：
	while (pBegin != pEnd) {
		cout << *pBegin << endl;
		pBegin++;
	}

	
	//第二种遍历方式：
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << endl;
	}
	cout << endl;

	//第三种遍历方式：
	//使用STL提供标准遍历算法  头文件 algorithm
	for_each(v.begin(), v.end(), MyPrint);
}

int main() {

	test01();

	system("pause");

	return 0;
}
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

#### 2.5.2 Vector存放自定义数据类型

学习目标：vector中存放自定义数据类型，并打印输出

**示例：**

```cpp
#include <vector>
#include <string>

//自定义数据类型
class Person {
public:
	Person(string name, int age) {
		mName = name;
		mAge = age;
	}
public:
	string mName;
	int mAge;
};
//存放对象
void test01() {

	vector<Person> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);
	Person p5("eee", 50);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);

	for (vector<Person>::iterator it = v.begin(); it != v.end(); it++) {
		cout << "Name:" << (*it).mName << " Age:" << (*it).mAge << endl;

	}
}


//放对象指针
void test02() {

	vector<Person*> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);
	Person p5("eee", 50);

	v.push_back(&p1);
	v.push_back(&p2);
	v.push_back(&p3);
	v.push_back(&p4);
	v.push_back(&p5);

	for (vector<Person*>::iterator it = v.begin(); it != v.end(); it++) {
		Person * p = (*it);
		cout << "Name:" << p->mName << " Age:" << (*it)->mAge << endl;
	}
}


int main() {

	test01();
    
	test02();

	system("pause");

	return 0;
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374
```

#### 2.5.3 Vector容器嵌套容器

学习目标：容器中嵌套容器，我们将所有数据进行遍历输出

**示例：**

```cpp
#include <vector>

//容器嵌套容器
void test01() {

	vector< vector<int> >  v;

	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	vector<int> v4;

	for (int i = 0; i < 4; i++) {
		v1.push_back(i + 1);
		v2.push_back(i + 2);
		v3.push_back(i + 3);
		v4.push_back(i + 4);
	}

	//将容器元素插入到vector v中
	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);
	v.push_back(v4);


	for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++) {

		for (vector<int>::iterator vit = (*it).begin(); vit != (*it).end(); vit++) {
			cout << *vit << " ";
		}
		cout << endl;
	}

}

int main() {

	test01();

	system("pause");

	return 0;
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

## 3.STL常用容器

### 3.1 string容器
```c++

```
### 3.2 vector容器
```c++

```
### 3.3 deque容器
```c++

```
### 3.4 案例-评委打分

```c++

```
### 3.5 stack容器
```c++

```
### 3.6 queue容器
```c++

```
### 3.7 list容器
```c++

```
### 3.8 set/multiset 容器
```c++

```
3.9 map/multimap容器

```c++

```
### 3.10 案例-员工分组
```c++

```
# 4.STL函数对象

## 4.1 函数对象
```c++

```
## 4.2 谓词
```c++

```
## 4.3 内建函数对象
```c++

```
# 5.STL常用算法

## 5.1 常用遍历算法
```c++

```
## 5.2 常用查找算法
```c++

```
## 5.3 常用排序算法
```c++

```
## 5.4 常用拷贝和替换算法
```c++

```
## 5.5 常用算术生成算法
```c++

```
## 5.6 常用集合算法
```c++

```



