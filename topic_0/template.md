# templete回顾
## 1.类模板
### 类模板实例化时并不是每个成员函数都实例化了, 而是使用到了哪个成员函数, 那个成员函数才实例化。
```cpp
#include<iostream>
using namespace std;
template<class T>
class point{
public:
    point(T x,T y):
        x(x),y(y){}
    void setX() const{//这里编译通过了
        x=y;
    }
private:
    T x,y;
};
int main(){
    return 0;//这里没有调用class，因此也没有报错
}
//因此可以看出，泛型类只有在调用后才会被实例化，因此g++在编译时候是检查不到的
//而普通的宏定义直接报错了
#include<iostream>
#define T int
using namespace std;
class point{
public:
    void setX() const{
        x=y;//这里直接报错，”表达式必须是可修改的左值“
    }
private:
    T x,y;
};
int main(){
    return 0;
}
```
### 可以把类模板和函数模板结合起来
```cpp
#include<iostream>
using namespace std;
template<class T>
class node{
public:
    template<class U>
    void _println(T x,U y){
        cout<<x<<" "<<y<<endl;
    }
};
int main(){
    return 0;
}
```
### 每个不同模板类型的实例都会有独有的static变量
```cpp
#include<iostream>
using namespace std;
template<class T>
class node{
public:
    static T x;
}; 
template<class T>
T node<T>::x=0;
int main(){
    node<int> t1,t2;
    node<double> t3;
    t1.x=1;
    cout<<t1.x<<endl;
    cout<<t2.x<<endl;
    cout<<t3.x<<endl;
    return 0;
}
/**
 * @brief 
 * 1
 * 1
 * 0
 * 输出结果
 */
```