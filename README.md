# 4. 内存问题

## 垃圾回收
* 标记清除: 在当前环境创建的变量标记为`进入环境`, 变量离开环境后标记为`离开环境`, 离开环境的值被自动标记为可回收, 因此将在垃圾收集期间被删除
* 引用计数: 记录每个变量被引用的次数, 当改变量不再被引用了引用次数减一, 现有JS引擎目前都不再使用这种算法, 但在IE中访问非原生JS对象(如DOM元素)时, 这种算法仍然可能会导致问题

> 引用计数存在问题: `循环引用`

## 内存性能问题:
> 确保占用最少的内存, 可以让页面获得更好的性能, 最佳方式: 执行时只保留有用的数据, 一但数据不再有用, 最好通过把这个变量指向null来释放内存, 这个方法叫解除引用

```
    基本类型和引用类型值的特点
    
    1. 基本类型在内存中占据固定大小的空间, 因此被保存在栈中
    2. 从一个变量向另一个变量复制类型的值, 会创建这个值的一个副本
    3. 引用类型的值是对象, 保存在堆中
    4. 包含引用类型的值的变量实际上包含的并不是对象本身, 而是一个指向该对象的指针
    5. 从一个变量向另一个变量复制引用类型的值, 复制的其实是指针, 最终两个变量指向同一个对象
    6. 确定一个值是哪种基本类型, 可以用typeof操作符, 而确定一个值是哪种引用类型, 可以使用instanceof操作符

```


# 5. 引用类型
创建Object类型的两种方法
```
1. var pereson = new Object()

2. var person = {
       name : "Nicolas",
       age : 29
   }
```

## 5.2 数组类型
创建数组的两种方法:
```
    1. var colors = new Array();

       var colors = new Array(20); //指定数组大小, 数组属性length会变成20

       var names = new Array("richard");

    2. var colors = [1, 2, 3]
```

* 5.2.2  转换方法:

    * toSting : 返回object 的字符串表示
    * valueof : 返回Object本身(特例: Date 返回与1970.1.1 的时间差)

* 5.2.3 栈方法:
可以用push 和pop 来操作数组

* 5.2.4 队列方法:
使用shift() 和 push() 方法, shift()移除数组第一个元素, push()在数组末尾添加一个元素

* sort, reverse:
    * 仅需要倒序时, reverse更快, sort 和 reverse操作会影响原数组
* 5.2.6 操作方法: 
    * concat 创建当前数组的副本, 对接收的参数添加到这个副本的末尾, 对于数组的参数, 数组参数中的每个值添加到副本末尾
    * slice: 根据下标范围截取原数组, 不会影响原数组, 如:
    ```
        var colors = ["red", "green", "blue", "yellow"]
        color1 = colors.slice(1, 4) // green, bluem yellow
    ```
    * splice:
        1. 删除: 指定下表范围, 删除任意数量的项. 例如, splice(0, 2)会删除数组的前两项
        2. 插入, 可以向指定位置插入任意数量的项, 提供3个参数: 起始位置, 0(要删除的项数), 和要插如的项, 例子:splice(2, 0, "red", "green")
        3. 替换: 可以向指定的位置插入任意数量的项, 而且同时删除任意数量的项, 参数和2一样
* 5.2.8 迭代方法:
    * every(): 传进一个函数, 如果每个元素进过这个函数之后都返回true, 那么every返回true

    * filter(): 对数组的每一项运行给定函数, 返回该数组会返回true的项组成的数组

    * map(): 对数组中的`每一项`执行给定函数, 返回所有的执行结果组成的数组

    * some(): 给数组传一个函数, 只要数组中有一个item进过这个函数后返回true, 那some就返回true

* 5.2.9 归并方法
    * reduce(): 从第一项开始, 向最后一项, 下标递增执行
    * reduceRight(): 从最后一项开始, 向第一项, 下标递减执行 




## 5.4. RegExp类型
```
    var expression = /pattern/flag

    // flag 类型如下
    g: 全局模式, 匹配全部符合的字符串, 而非在发现第一个匹配项后立即停止

    i: 表示不区分大小写模式
    m: 表示多行模式(multiline), 即使在达到一行文本的末尾, 还会继续查找下一行是否匹配

    /*
     * 匹配字符串中所有"at"的实例
     */
    var pattern1 = /at/g;

    /*
     * 匹配第一个"bat"或"cat", 不区分大小写
     */
    var pattern2 = /[bc]at/i;

    /*
     * 匹配全部以"at"结尾的三个字符的组合, 不区分大小写
     */
    var pattern3 = /.at/gi;


    /*
     * 与pattern3相同, 只是使用了构造函数来创建
     */
    var pattern4 = new RegExp(".at", "gi")
```
> 匹配以某字符串结尾的字符串, 可参照以下例子
```
    > s = "ccabbsbbbcbabbbwebbabdabbbbqwbebabbabba"
    'ccabbsbbbcbabbbwebbabdabbbbqwbebabbabba'

    > s.split(/[^\a]+/);
    [ '', 'a', 'a', 'a', 'a', 'a', 'a', 'a' ]
    
    > s.match(/[^\a]+/g);
    [ 'cc', 'bbsbbbcb', 'bbbwebb', 'bd', 'bbbbqwbeb', 'bb', 'bb' ]
```

## 5.5: Function 类型
> 函数实际上是对象, 每个函数都是Function类型的实例, 而且都与其他引用类型一样具有属性和方法.

函数就是对象, 函数名就是指针
```
var sum = function(num1, num2){
    return num1+num2
};

等价于
var sum = new Function("num1", "num2", "return num1+num2);
// 最下面的方法不推荐, 因为会产生两次解析
```
* 5.5.3  作为值的函数:
    * 去掉括号, 例如 add, 表示这个函数对象的引用

    * 保留括号, 例如 add(), 表示对这个函数对象的调用

* 5.5.4 函数的内部属性

    * arguments ,常规用法easy, 以下为重点用法callee, 用于解耦
    ```
    function factorial(num){
        if(num <= 1){
            return 1;
        } else{
            return num * arguments.callee(num-1)
            // 耦合写法是 num * factorial(num-1), 如果换个函数名, 这种写法就不行了, 所以要用上面的写法, 可以解耦
        }
    }
    ```
    * ## `this`(重要!): 用法跟python的self差不多
        * this会动态绑定对象(如果函数中包含this, 而且该函数指针被赋值给了别的对象的函数)


*   5.5.5 函数属性和方法
    
    * prototype: 保存 `引用类型` 的 `实例方法` 的真正所在, 无法用for-in枚举, 在创建自定义 `引用类型` 时要特别注意prototype

    * `apply`: (重要!) 注意, this的内容是什么

    * call : 和apply一样, 只是第二个参数不能传入数组, 传入确定参数



# 模块化编程:


# 6. 面向对象的程序设计

## 6.1.1 属性类型

> ECMAScript包含两种属性: `数据属性`和`访问器属性`. 为了表示特性是内部值, ES5规范把内部值放在两对方括号里, 如[[Enumerable]].

1. 数据属性, 有4个描述其行为的特性
    * [[Configurable]]:  是否可以删除属性重新定义, 是否能修改属性的特性
    
    * [[Enamerable]]: 是否可枚举
    
    * [[Writable]]: 是否能修改属性的值

    * [[Value]]: 包含这个属性的数据值