---
layout:     post
title:      "Lambda Expressions"
subtitle:   "Learn Lambda"
date:       2016-08-06 14:18:00
author:     "Jayden"
header-img: "img/post-bg-2015.jpg"
tags:
    - Java
	- Lambda
---

# Lambda

> �����ڲ����Ѿ���һ�ּ��ı�ʾ�����ˣ����ǻ���һ�����⣬����һ���ӿڣ���ֻ����һ����������ô�����ڲ���Ͳ���ô����ʹ���ˣ���Ϊ����ֺܶ�����Ĵ��룬���ʱ�򿪷��߾ͻ���Ҫ����������һ���������ݸ�һ������������Ϊ�ؼ����õ���¼������Ծͳ�����Lambda���ʽ��Java8��������������ԣ�ʹ��Android Studio���ʱ���ᷢ��Ϊ�ؼ����õ���¼��Ĵ���ᡰ�����������µڶ��ֱ�ʾ��ʽ����Lambda���ʽ����Ȼ�ٷ����������ˣ������ǻ���ʲô���ɲ�ӵ�����ַ�ʽ�ء�

```
 view.setOnClickListener(new View.OnClickListener() {//����������д�� 
            @Override
            public void onClick(View v) {
                
            }
        });
        
  view.setOnClickListener((v) -> {//�е�ʱ����뿴��ȥ���������

        });  
```


- �÷�
    - [Android Studio���ã�](https://developer.android.com/preview/j8-jack.html#supported-features)
    
        ```
        android {
            buildToolsVersion "24.0.0"//����24����
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
    - �﷨
        - Lambda���ʽֻ������ֻ����һ�������Ľӿ�
        - Lambda���ʽ�Ľṹ��(Type params) -> {block}���������ʽ������ǡ��ӿڵ�ʵ���ࡱ��params����������ӿ��еķ������βΡ���Type�����βε����͡�������ʡ�ԣ�block�����ӿ��з�����ʵ�֡��������뿴��������ӣ�
        
        ```
           public interface ISyntax {//Lambda���ʽֻ������ֻ����һ�������Ľӿ�
            int getInt(int i);
        }
    
        private void invokeSyntax(ISyntax s) {//�������������Ҫ����ISyntax��ʵ����
            s.getInt(10);
        }
    
        private void learnSyntax() {
            //�����ڲ����Ѿ�ʡ������������������裬�������ܼ���ˡ�
            invokeSyntax(new ISyntax() {
                @Override
                public int getInt(int i) {
                    return i++;
                }
            });
    
             // Lambda���ʽ��ʵҲ�������ģ�ֻ�������������˶������ƣ�������������������
            invokeSyntax((int i) -> { //�����int i���ǽӿ�ISyntax�е�getInt()�����еĲ���
                return i++; //�������е����ݾ���getInt()������ʵ��
            });
    
            invokeSyntax((i) -> i++);//����ʡ�Բ������ͣ�������ʡ�Դ����ź͡�return����ֱ��д����ֵ
        }
        ```
        
        - ������
        
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
        
        - ��������
            
            Lambda���ʽֻ�����β��б��ʵ�ִ���飬��ôJava����ж�һ��Lambda���ʽ����ľ������ĸ��ӿڵ�ʵ�����أ�����������ӣ�
            
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
                use((int i) -> {//��Lambda�������IInteger��ʵ���࣬��Ϊ����������Ϊint����
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
                use((String str) -> "���з���ֵ" + str);
            }
            ```
           �����ܽ������¼����ж�Target Type��Ŀ�����ͣ��ķ���
           
            - Variable declarations ��������
    
            - Assignments   
    
            - Return statements     ����ֵ����
            
            - Array initializers    �����ʼ��
            
            - Method or constructor arguments   �������캯������
            
            - Lambda expression bodies  Lambda���ʽ�����
            
            - Conditional expressions, ?: �������ʽ
            
            - Cast expressions  
            
    - �ŵ�
        - Lambda���ʽ��ʵ����������������࣬ȥ����û��ʵ������Ĵ��룻
    - ȱ��
        - ���Ҳ������ȱ�㣬ʹ��Lambda���ʽ����Ҫ�Ժ����ǳ�����Ϥ��֪��������ʲô������
        
>  �ο����£�[Oracle����](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html#syntax)��[Android��������վ](https://developer.android.com/preview/j8-jack.html#supported-features)��