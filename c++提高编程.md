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
#include <vector>

void printVector(vector<int>& v) {

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//插入和删除
void test01()
{
	vector<int> v1;
	//尾插
	v1.push_back(10);
	v1.push_back(20);
	v1.push_back(30);
	v1.push_back(40);
	v1.push_back(50);
	printVector(v1);
	//尾删
	v1.pop_back();
	printVector(v1);
	//插入
	v1.insert(v1.begin(), 100);
	printVector(v1);

	v1.insert(v1.begin(), 2, 1000);
	printVector(v1);

	//删除
	v1.erase(v1.begin());
	printVector(v1);

	//清空
	v1.erase(v1.begin(), v1.end());
	v1.clear();
	printVector(v1);
}

int main() {

	test01();

	system("pause");

	return 0;
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
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
#include <vector>

void test01()
{
	vector<int>v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}

	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1.at(i) << " ";
	}
	cout << endl;

	cout << "v1的第一个元素为： " << v1.front() << endl;
	cout << "v1的最后一个元素为： " << v1.back() << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
12345678910111213141516171819202122232425262728293031323334
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
#include <vector>

void printVector(vector<int>& v) {

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	vector<int>v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2;
	for (int i = 10; i > 0; i--)
	{
		v2.push_back(i);
	}
	printVector(v2);

	//互换容器
	cout << "互换后" << endl;
	v1.swap(v2);
	printVector(v1);
	printVector(v2);
}

void test02()
{
	vector<int> v;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
	}

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	v.resize(3);

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	//收缩内存
	vector<int>(v).swap(v); //匿名对象

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;
}

int main() {

	test01();

	test02();

	system("pause");

	return 0;
}

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566
```

总结：swap可以使两个容器互换，可以达到实用的收缩内存效果

#### 3.2.8 vector预留空间

**功能描述：**

- 减少vector在动态扩展容量时的扩展次数

**函数原型：**

- `reserve(int len);`//容器预留len个元素长度，预留位置不初始化，元素不可访问。

  

**示例：**

```cpp
#include <vector>

void test01()
{
	vector<int> v;

	//预留空间
	v.reserve(100000);

	int num = 0;
	int* p = NULL;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
		if (p != &v[0]) {
			p = &v[0];
			num++;
		}
	}

	cout << "num:" << num << endl;
}

int main() {

	test01();
    
	system("pause");

	return 0;
}
123456789101112131415161718192021222324252627282930
```

总结：如果数据量较大，可以一开始利用reserve预留空间

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
### 3.9 map/multimap容器

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



