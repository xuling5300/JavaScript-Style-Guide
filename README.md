---
title: JavaScript编码风格指南 
tags: 
---
*记录下在学习javascript过程中的心得体验，希望能提高自己在编码方面的能力和高度，或者如果你也看到这篇文章，也能对你有帮助。*




**1. 使用测试套件**
-------------
曾经在一开始进行开发项目的时候就开始担心后面项目越来越大，越来越复杂，该怎么测试呢。人工？额，一个正常中等大小的项目应该至少需要一个专门的测试小组才能完成这个工作吧。于是在公司还没有财力给项目专门配备测试小组的时候，项目就已经稀稀拉拉开始进行了。再于是，项目越来越失去控制，你可以想象，有效的测试就根本无法开展，最多只能做做简单的功能性测试，和场景流程测试。
有幸看了本书《JavaScript应用程序设计》（作者Eric Elliott），于是对JS的自动化测试有了概念。编写测试用例会让你对功能实现了解的更为透彻，对方案与接口设计的把握越来越谨慎，设计测试用例的过程同样是迫使你思考将代码解耦的过程。保持编写单元测试与解耦代码的习惯会使你在今后的工作中持续收益。
废话这么多。好吧，市面上的JS测试框架还是很多的，这里接触到的是QUnit，其他类似框架还有Jasmine、Jarvis、jfUnit等，有空再研究。
QUnit非常简单易学，从来没接触过的人，几乎5分钟就能掌握它的使用方法。如果将单元测试引入到你的项目中，我想这对整个项目的开发效率和质量将有不小的收效。

QUnit常用函数：
----------

- **module()**
module()函数用于将相关的测试用例归类后输出在测试报告中，参数可以是任意字符，语法：
    
        module('Name');

- **test()**
test()函数用于定义与命名一组测试用例，包含了所有需要的断言，语法：

        test('Name', function(){
            //your assertion here...
        })

- **ok()**
     ok()函数可以测试表达式返回值的真伪，语法：

        ok(true, 'Description')
    
- **equal()**
     equal()函数用于比较两个值之间是否相等，语法：

        var a = true;
        equal(a, true, 'a should be true');
    

**2. 最佳编码实践**
-------------

 **1. 缩进需保持一致**
 这是一个良好的编码习惯，保持一致的缩进会让代码看起来更易读，更整洁。谁愿意去看一堆格式杂乱的代码呢？

 **2. 分号的使用**
    书中推荐的方法是将程序中所有可选的分好移除，我暂时还没有这方面的领悟，还是老老实实该分号就分号，以免出错。
    
 **3. 将花括号放置在右边**
 
避免这样使用：
 
        var test = function test()
        {
            //balabala
        };

推荐这样使用：

        var test = function test(){
            //balabala
        };

 **4. 避免命名冲突**
 JavaScript中的变量作用域默认为全局，为了避免变量命名冲突，应用中全局变量的数量应当尽可能少。通常情况下使用一个全局对象作为应用自身的命名空间使用，其余的命名空间则留给一些第三方类库，如JQuery，Underscore等。
避免这样使用：

        var x;
        //balabala
    
推荐这样使用：

        (function myScript(){
            var x;
            //balabala
        }());

 **5. 使用var关键字**
 如果你在变量声明时忘记了使用var关键字，那么所声明的变量将会直接污染全局作用域。
避免这样使用：

        var add = function add(number){
            x = 2;  //x被声明为全局变量
            return number + x;
        }
    
推荐这样使用：

        var add = function add(number){
            var x = 2;
            return number + x;
        }

 **6. 在函数体中仅使用一个var语句**
 避免这样使用：
 
        (function myScript(){
            var x = true;
            var y = true;
            var z = true;
        }());
        
推荐这样使用：

        (function myScript(){
            var x = true,
                y = true,
                z = true;
        }());
        
我并没有很理解这样使用的好处，不过书上说是因为JavaScript有变量提升的特性，这种使用方法会避免在变量提升时出现异常。简单的解释变量提升的概念就是，如果你在全局作用域声明了变量x。然后在函数内部也声明一次变量x，但是在声明前你就使用了变量x，这时候它其实是仍然未被定义的（`undefined`）。
JavaScript实际上是这样解释这中情况的：

        var x = 1;
        
        (function(){
            var x;
            console.log(x); //x is undefined
            x = 2;
            console.log(x); //x = 2
        }());

**7. 避免常量滥用**
书中在这部分的总结很实用。当一个项目逐渐变得越来越复杂时，在开始编码之前就应应仔细规划和设计好项目的组织结构。
避免这样使用：

        var createUserBox = function createUserBox(){
            var WIDTH = '80px',
                HEIGHT = '80px';
                
            return {
                width: WIDTH,
                height:HEIGHT
            };
        },
    
        createMenuBox = function createMenuBox(){
            var WIDTH = '200px,'
                HEIGHT = '200px';
                
            return {
                width : WIDTH,
                height: HEIGHT
            }
        };
        
        var userBox = createUserBox(),
            menuBox = createMenuBox();
            
        //balabala

推荐这样使用：

        var createBox = function createBox(option){
            var defaults = {
                width : '80px',
                height: '80px'
            };
            
            return $.extend({}, defaults, option);
        };
        
        var userBox = createBox({
            width : '80px',
            height: '80px'
        }),
        
        var menuBox = createBox({
            width : '200px' ,
            height: '200px'
        });
        
上面的写法瞬间觉得很清晰了。

**8. 尽可能多的使用函数迭代器**
 你还停留在喜欢用for循环来做迭代？下面两种写法你要好好学习啦：
较好的版本：
    
        var getCount = function getCount(){
            var count = [1, 2, 3],
                text = '';

            count.forEach(function(number){
               text += number + ''; 
            });
            
            return text;
        };
    更好的版本：

        var getCoung = function getCount(){
            var count = [1, 2, 3];
            
            return count.reduce(function(previous, number){
                return previous + number + ' ';
            }, '');
        };

**9. 留心if语句**
 在使用if语句时经常会使用到表达式，这些常见值可能容易被你忽略，但是它往往很重要。布尔值为false的常见值包括：
    - 0
    - undefined
    - null
    - 空字符串
    - false
    所有内容为空对象或数组的布尔值都视为true，包括像`new Boolean(false)`这样的对象.

**10. 规避副作用**
 尽可能不要对在函数外声明的对象做任何变更操作，这样做可以使你的代码更为健壮与复用。

避免这样使用：

        var x = 0;
        
        function increment(){
            x += 1; //全局变量x的值会被修改
            return x;
        }
        
推荐这样使用：
    
        var x = 0;
        
        function increament(x){
            x += 1; //x值得修改仅作用于函数内部
            return x;
        }
        
避免这样使用：

        var obj = {
            value : 2
        };
        
        function setValue(obj, value){
            obj.value = value; //obj对象的value属性会被修改
            return obj;
        }
        
        console.log(setValue(obj,3)); //Object {value : 3}
        
推荐这样使用：

        var obj = {
            value : 2
        };
        
        function setValue(obj, value){
            var instance = $.extend({}, obj); //extend是jquery的扩展方法
            return instance;
        }
        console.log setValue(obj, 3); //Object (value :2)
        
        
**11. 少用switch**
 在switch的case语句块中如果不使用break关键字，那么它还会接着跳进下一个case中执行。根据逻辑的需要使用或者不使用break关键字，但是在实际中有时会比较难以控制，代码会变得难以维护。所以能不用还是不要用的好。




