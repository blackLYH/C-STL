# C++中STL容器寻找最大最小值方法
如果存在一个容器vector，在不进行排序的情况下使用stl函数寻找最大值最小值，则只需要如下操作：
### 1. 寻找最大值
*    std里面有max_element函数，返回最大值对应的迭代器，跟其他的stl函数一样，需要传入该函数对此容器的适用范围，使用时需要include<algorithm> 头文件。使用示例：
```C++
#include <algorithm>
#include <iostream>
#include <vector>
int main(){
    int a[6] = {15,2,3,36,22,6};// random shuffle
    std::vector<int> v(a,a+6); // initial the vector
    std::vector<int>::iterator it = std::max_element(v.begin(),v.end()); // this function return a iterator which point to the max element
    std::cout<<"the max element is "<< *it<<std::endl; //print the result
    
    return 0;
}
```
输出：
```
the max element is 36
```

### 2. 寻找最小值  
*    同寻找最大值，使用min_element函数即可，使用示例：
```C++
#include <algorithm>
#include <iostream>
#include <vector>
int main(){
    int a[6] = {15,2,3,36,22,6};// random shuffle
    std::vector<int> v(a,a+6); // initial the vector
    std::vector<int>::iterator it = std::min_element(v.begin(),v.end()); // this function return a iterator which point to the min element
    std::cout<<"the min element is "<< *it<<std::endl; //print the result
    
    return 0;
}
```
输出：
```
the min element is 2
```
### 3. 拓展  
*    对于非C++典型类型（int,double,float等）排序，可以自定义一种排序函数，然后亦可用STL函数寻找最大最小值。如以下示例将通过坐标点的x值进行排序：
```C++
#include <algorithm>
#include <iostream>
#include <vector>
// define the point struct 
struct Point2f{
    float x;
    float y;
};
// define the compare function
// if a.x < b.x return true
// if a.x == b.x ,return which y is smaller
inline bool comp(const Point2f &a,const Point2f &b){
    return a.x<b.x ||(a.x == b.x && a.y < b.y);
}

// define the output function
std::ostream& operator<<(std::ostream &os,const Point2f &p){
    os<<"["<<p.x<<","<<p.y<<"]";
    return os;
}
int main(){
    std::vector<Point2f> v; // define the vector
    v.push_back(Point2f{2.0f,4.1f});
    v.push_back(Point2f{2.8f,5.1f});
    v.push_back(Point2f{6.8f,8.2f});
    v.push_back(Point2f{1.0f,3.0f});
    v.push_back(Point2f{1.0f,2.0f});
    std::vector<Point2f>::iterator Minit = std::min_element(v.begin(),v.end(),comp); // this function return a iterator which point to the min element
    std::vector<Point2f>::iterator Maxit = std::max_element(v.begin(),v.end(),comp); // this function return a iterator which point to the max element
    std::cout<<"the min element is "<< *Minit<<std::endl; //print the result
    std::cout<<"the max element is "<< *Maxit<<std::endl; //print the result
    return 0;
}
```
输出：
```
the min element is [1,2]
the max element is [6.8,8.2]
```

### 4. 返回第n大的值/第n小的值  

*    在实际应用中，我们不一定需要找到最大值或者最小值，而需要寻找第n大的值或者第n小的值。这种实际应用在寻找中位数等。传统做法通常需要先进行排序，然后根据排序寻找相应的值。但是在C++的STL里面给了另一种更快捷的函数—— nth_element。它不需要对整个容器进行完全排序，而是进行部分排序即可得到。  
    它的原型是：  
    **nth_element(first,nth,last);** 
```
    其中first和last是指该函数对此容器的适用范围   
    nth是要定位的第n个元素，能对它进行随机访问。  
    这三个参数都是容器对应的迭代器。
```
使用示例：
```C++
#include <algorithm>
#include <iostream>
#include <vector>
int main(){
    int a[6] = { 15, 2, 3, 36, 22, 6 };// random shuffle
    std::vector<int> v(a, a + 6); // initial the vector
    int nth = 3; // test to find the 3rd small element
    std::nth_element(v.begin(), v.begin() + nth-1, v.end()); // this function return a iterator which point to the nth small element
    std::cout << "the nth small element is " << *(v.begin() + nth - 1) << std::endl; //print the result

    return 0;
}
```
输出：
```
the nth small element is 6
```

而如果我们需要第n大的值，可以先通过得到容器的size，进行反推算，比如：容器内有10个元素，我们需要找到第2大的值，那么也就是第9小的值。通过求第9小的值去得到第2大的值。示例：
```C++
#include <algorithm>
#include <iostream>
#include <vector>
int main(){
    int a[10] = {15,2,3,36,22,6,8,94,20,35};// random shuffle
    std::vector<int> v(a,a+10); // initial the vector
    int nth = 2; // test to find the 2nd big element
    std::nth_element(v.begin(),v.begin()+(v.size()-nth),v.end()); // this function return a iterator which point to the nth big element
    std::cout<<"the nth big element is "<< *(v.begin()+(v.size()-nth))<<std::endl; //print the result
    
    return 0;
}
```
输出：
```
the nth samll element is 36
```
当然可以参考3的拓展。nth_element也提供自定义比较函数去寻找对应的值。比如上面的寻找第二大值示例:
```C++
#include <algorithm>
#include <iostream>
#include <vector>
// define this function to find the greater element
inline bool comp(const int &a,const int &b){
    return a>b;
}
int main(){
    int a[10] = {15,2,3,36,22,6,8,94,20,35};// random shuffle
    std::vector<int> v(a,a+10); // initial the vector
    int nth = 2; // test to find the 2nd big element
    std::nth_element(v.begin(), v.begin() + nth-1, v.end(),comp); // this function return a iterator which point to the nth big element
    std::cout << "the nth big element is " << *(v.begin() + nth - 1) << std::endl; //print the result

    return 0;
}
```
输出:
```
the nth big element is 36
```
**注意：** nth_element函数会把容器内的顺序打乱，会把小于第n个的放到n位置的前面，把大于第n个的放到n位置的后面，但是不一定有序。

### 5. 返回最大值/最小值的下标
* 有时候我们并不需要得到最大值或者最小值，而是想得到最大值或最小值所对应的下表即可。那么我们可以使用STL的distance函数。  
**distance(firstIterator,lastIterator);**  
distance函数原意是计算firstIterator到lastIterator之间的距离，参数类型是迭代器类型。我们可以用来计算最大值/最小值对应的下标（下标从0开始）。使用示例：
```C++
#include <algorithm>
#include <iostream>
#include <vector>

int main(){
    int a[6] = { 15, 2, 3, 36, 22, 6 };// random shuffle
    std::vector<int> v(a, a + 6); // initial the vector
    std::vector<int>::iterator maxIt = std::max_element(v.begin(), v.end()); // this function return a iterator which point to the max element
    std::vector<int>::iterator minIt = std::min_element(v.begin(), v.end()); // this function return a iterator which point to the min element
    
    int maxIndex = std::distance(v.begin(),maxIt); // the index of max element
    int minIndex = std::distance(v.begin(),minIt); // the index of min element
    
    std::cout << "the index of max element is " << maxIndex << std::endl; //print the result
    std::cout << "the index of min element is " << minIndex << std::endl; //print the result
    
    return 0;
}
```
输出：
```
the index of max element is 3
the index of min element is 1
```
**注意**：这里的下标是指从0开始计算。即下标3代表第四个值。
