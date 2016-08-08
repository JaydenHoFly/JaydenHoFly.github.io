---
layout:		post
title:		"Lambda Expressions"
subtitle:	"Learn Guava"
date:		2016-08-06
author:		"JaydenHo"
header-img: "img/post-bg-2015.jpg"
tags:
    - Java
    - Guava
    - Lambda
---

# Lambda 

> 匿名内部类已经是一种简洁的表示方法了，但是还有一个问题，例如一个接口，它只含有一个方法，那么匿名内部类就不那么易于使用了，因为会出现很多冗余的代码，这个时候开发者就会想要将函数当成一个参数传递给一个方法，例如为控件设置点击事件。所以就出现了Lambda表达式，Java8引入了这个新特性，使用Android Studio编程时，会发现为控件设置点击事件的代码会“变样”，如下第二种表示方式就是Lambda表达式，既然官方都这样用了，那我们还有什么理由不拥抱这种方式呢。

```
 view.setOnClickListener(new View.OnClickListener() {//明明是这样写的
            @Override
            public void onClick(View v) {
                
            }
        });
        
  view.setOnClickListener((v) -> {//折叠代码后看上去变成了这样

        });  
```


- 用法
    - [Android Studio配置：](https://developer.android.com/preview/j8-jack.html#supported-features)
    
        ```
        android {
            buildToolsVersion "24.0.0"//大于24即可
            defaultConfig {
                jackOptions {
                    enabled true
                }
            }
            compileOptions {
                sourceCompatibility JavaVersion.VERSION_1_8
                targetCompatibility JavaVersion.VERSION_1_8
            }
        }
        ```
    - 语法
        - Lambda表达式只适用于只含有一个方法的接口
        - Lambda表达式的结构：(Type params) -> {block}，整个表达式代表的是“接口的实现类”；params代表这个“接口中的方法的形参”；Type代表“形参的类型”，可以省略；block代表“接口中方法的实现“，具体请看下面的例子；
        
        ```
           public interface ISyntax {//Lambda表达式只适用于只含有一个方法的接口
            int getInt(int i);
        }
    
        private void invokeSyntax(ISyntax s) {//调用这个方法需要传递ISyntax的实现类
            s.getInt(10);
        }
    
        private void learnSyntax() {
            //匿名内部类已经省略了声明对象这个步骤，看起来很简洁了。
            invokeSyntax(new ISyntax() {
                @Override
                public int getInt(int i) {
                    return i++;
                }
            });
    
             // Lambda表达式其实也是匿名的，只不过不仅仅匿了对象名称，还匿了类名，方法名
            invokeSyntax((int i) -> { //这里的int i就是接口ISyntax中的getInt()方法中的参数
                return i++; //大括号中的内容就是getInt()方法的实现
            });
    
            invokeSyntax((i) -> i++);//可以省略参数类型，还可以省略大括号和“return”，直接写返回值
        }
        ```
        
        - 作用域
        
        ```
        import java.util.function.Consumer;
    
        public class LambdaScopeTest {
    
            public int x = 0;
        
            class FirstLevel {
        
                public int x = 1;
        
                void methodInFirstLevel(int x) {
                    
                    // The following statement causes the compiler to generate
                    // the error "local variables referenced from a lambda expression
                    // must be final or effectively final" in statement A:
                    //
                    // x = 99;
                    
                    Consumer<Integer> myConsumer = (y) -> 
                    {
                        System.out.println("x = " + x); // Statement A
                        System.out.println("y = " + y);
                        System.out.println("this.x = " + this.x);
                        System.out.println("LambdaScopeTest.this.x = " +
                            LambdaScopeTest.this.x);
                    };
        
                    myConsumer.accept(x);
        
                }
            }
        
            public static void main(String... args) {
                LambdaScopeTest st = new LambdaScopeTest();
                LambdaScopeTest.FirstLevel fl = st.new FirstLevel();
                fl.methodInFirstLevel(23);
            }
        }
        This example generates the following output:
        
        x = 23
        y = 23
        this.x = 1
        LambdaScopeTest.this.x = 0
        ```
        
        - 返回类型
            
            Lambda表达式只含有形参列表和实现代码块，那么Java如何判断一个Lambda表达式代表的究竟是哪个接口的实现类呢？看下面的例子：
            
            ```
             //Target Type
            public interface IInteger {
                void setInt(int i);
            }
        
            public interface IString {
                void setString(String str);
            }
        
            public interface IMultiParams {
                void setString(String str1, String str2);
            }
        
            public interface IReturn {
                String getString(String str);
            }
        
            private void use(IInteger e) {
                e.setInt(1);
            }
        
            private void use(IString t) {
                t.setString("IString");
            }
        
            private void use(IMultiParams m) {
                m.setString("param1", "param2");
            }
        
            private void use(IReturn r) {
                r.getString("IReturn");
            }
        
            public void learnTargetType() {
                //IInteger
                use((int i) -> {//该Lambda代表的是IInteger的实现类，因为参数声明了为int类型
                    Log.d(TAG, "i: " + i);
                });
                //IString
                use((String str) -> {
                    Log.d(TAG, "str: " + str);
                });
                //IMultiParams
                use((param1, param2) -> {
                    Log.d(TAG, "param1: " + param1 + " param2: " + param2);
                });
                //IReturn
                use((String str) -> "我有返回值" + str);
            }
            ```
           官网总结了以下几点判断Target Type（目标类型）的方法
           
            - Variable declarations 变量声明
    
            - Assignments   
    
            - Return statements     返回值声明
            
            - Array initializers    数组初始化
            
            - Method or constructor arguments   方法或构造函数参数
            
            - Lambda expression bodies  Lambda表达式代码块
            
            - Conditional expressions, ?: 条件表达式
            
            - Cast expressions  
            
    - 优点
        - Lambda表达式其实就是匿名函数，简洁，去除了没有实际意义的代码；
    - 缺点
        - 这个也不算是缺点，使用Lambda表达式必须要对函数非常的熟悉，知道它含有什么参数；
        
>  参考文章：[Oracle官网](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html#syntax)、[Android开发者网站](https://developer.android.com/preview/j8-jack.html#supported-features)。