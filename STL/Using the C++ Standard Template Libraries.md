# Using the C++ Standard Template Libraries

标签（空格分隔）： STL C++

---

C++标准库的头文件的集合，被称为C++标准库或STL。
贯穿STL的泛型编程思想早在1979年起源于Alexander Stepanov——很久之前并没有C++语言标准。

将要深度讨论的主要标准库头文件包括：

 - 用于数据容器：<array><vector><deque><stack><queue><list><forward_list><set><unordered_set><map><unordered_map>
 - 用于迭代器：<iterator>
 - 用于算法：<algorithm>
 - 用于随机数和统计：<random>
 - 用于数值处理：<valarray><numeric>
 - 用于时间和定时：<ratio><chrono>
 - 用于复数：<complex>

**没有人可以只通过看书就学会编程。只有通过写代码才能学会使用STL**

STL是一个功能强大且可扩展的工具集，用来组织和处理数据。为了最大限度地满足各种类型数据的需求，这个工具集全部由模板定义。
STL可以被划分为4个概念库：

 1. **容器库(Containers Library)**定义了管理和存储数据的容器。这个库的模板被定义在以下几个头文件中：array,vector,stack,queue,deque,list,forward_list,set,unordered_set,map以及unordered_map.
 2. **迭代器库（Iterators Library）**定义了迭代器，迭代器是类似于指针的对象，通常被用于引用容器类的对象序列。这个库被定义在单个头文件iterator中。
 3. **算法库(Algorithms Library)**定义了一些使用比较广泛的通用算法，可以运用到容器中的一组元素上。这个库的模板被定义在algorithm头文件中。
 4. **数值库（Numerics Library）**定义了一些通用的数学函数和一些对容器元素进行数值处理的操作。这个库也包含一些用于随机数生成的高级函数。这个库的模板被定义在complex,cmath,valarray,numeric,random,ratio以及cfenv这些头文件中。


**模板**：是一组函数或类的参数实现。编译器能够在需要使用函数或类模板时，用模板生成一个具体的函数或类的定义，也可以定义参数化类型的模板。因此模板并不是可执行代码，而是用于生成代码的蓝图或配方。
从模板生成的函数或类的定义是模板的实例或实例化。模板的参数值通常是数据类型，所以一个函数或类的定义可以通过int类型的参数值生成，也可以通过string类型的参数值生成。


 
 
