---
layout: post
title: "��SpringBoot����Ŀ��������"
date: 2017-12-24 13:38:54 +0800
comments: true
categories: SpringBoot
tags: [SpringBoot]
keyword: �º���, ����, SpringBoot
description:  ��Ŀ��������
---


�򵥵Ľ���һ��SpringBoot���������á�

�����ϽڵĲ���ม�  

����application.properties�ļ�:  
```xml
#��һ�����÷�ʽ
#�������÷�ʽÿ��������������д����
server.port=8081
#���ö˿�
server.context-path=/hello
#������Ŀ·��
```

������һ�¿��������ʱ�������Ŀ��·����Ҫ����/hello��Ŀ���ˡ�  
���Ҷ˿���8081��Ĭ�ϵ���8080  

���еڶ������÷�ʽ֮ǰ����ɾ��application.properties�ļ�  
�ҾͲ�ɾ���ˣ���������Ϊapplication.txt�ļ��ˣ�������ɾ����ѡ��ڶ������÷�ʽ����  
#�ڶ������÷�ʽ-�Ƽ�

��resourceĿ¼���½��ļ�:application.yml  
```
server:
  port: 8081
#  :�ź�������пո�
  context-path: /hello
```
������þͷ���ܶ࣬����ȫ���ˡ�  
�и�ע������뿴�����е�ע��  

���н���͵�һ�����÷�ʽ��һ����  

#�����Զ�������ñ���
��������:
```
server:
  port: 8082
#  :�ź�������пո�
  context-path: /hello
name: �º���
age: 20
```
���ǲ���Ҫ���������ñ������ͣ�ֻҪ��ע���ʱ��д���������ͼ���  
����ʹ�õ��� @Valueע��  

�ڴ����ж�ȡ����:  
```java
package cn.chenhaoxiang;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

/**
 * Created with IntelliJ IDEA.
 * User: �º���.
 * Date: 2017/12/25.
 * Time: ���� 9:44.
 * Explain:
 */
@RestController
public class HelloController {

    @Value("${name}")//���������ȡд���е���jsp��ȡsession��
    private String name;

    @Value("${age}")
    private Integer age;

    @RequestMapping(value = "/hello",method = RequestMethod.GET)
    public String say() {
        return "Hello Spring Boot!";
    }

    @RequestMapping(value = "/info",method = RequestMethod.GET)
    public String info() {
        return name+","+age;
    }
}

```
����֮�󣬵�����������ַ�����н��  
![](https://i.imgur.com/UFB9qtj.png)    

��������������ʹ�����ã����ǿ����������ļ�����ôд:  
```
info: "name:${name},age:${age}"
```
�����Ϳ���������������name��ֵ��age��ֵ  

��û�з�����������÷�ʽ�е��鷳��������кܶ����ԣ�����Ҫд�ܶ��ȡ��д��  
���ģ��϶��м�㷽ʽ�ģ����ʱ�����ǿ���ѡ����������װ  

���Ƕ���һ��People�ࡣ  
�����䣬��������ַ����  
������������ɣ�����˵��һ��  
```java
package cn.chenhaoxiang;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

/**
 * Created with IntelliJ IDEA.
 * User: �º���.
 * Date: 2017/12/25.
 * Time: ���� 9:58.
 * Explain:
 */
@Component //ע��Bean��Ҫ
@ConfigurationProperties(prefix = "people")//��ȡǰ׺��people������
public class People {

    private String name;

    private Integer age;

    private String address;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "People{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", address='" + address + '\'' +
                '}';
    }
}

```

������������People��ֵ:
```
people:
  name: chx
  age: 20
  address: ��ɳ
```

HelloController.java
���ǿ�������ע��People��Bean��
```java
@Autowired
    private People people;

@RequestMapping(value = "/people",method = RequestMethod.GET)
public People people() {
    return people;
}//���ص��Ƕ����JSON�ַ���
```

���ǿ����:  
![](https://i.imgur.com/6qGz9zD.png)  


#��̬����
�������ǿ�����ʱ��ͷ�����ʱ��ʹ�õ����ݿ��ַ��ͬ�����ǿ�����������  

�½����������ļ����ֱ�Ϊ:
application-dev.yml  ����ʹ��
application-prod.yml  ����ʹ��  

Ĭ�ϵ������ļ��е����ݿ���ɾ���ˡ�д��:  
application.yml  
```
spring:
  profiles:
    active: dev
```
�޸�application-dev.yml�е�ֵ����application-prod.yml��ͬ���ɣ����ʱ�����������Ŀ�������ӣ����Կ���people��ֵ��dev�ļ�������  
����Խ�dev�ĳ�prod���������ݼ���prod�ļ��е�����  

�������������Ƕ�̬����Ϊ������Ҫÿ�θı�application.yml�е�ֵ��  
�������ǿ���������������ƪ���͵�������ʽ  
Ҳ����java -jar��������ʽ  

���ȱ���һ�£�
```
mvn install
```
Ȼ������:
```
java -jar target/hello-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod
```
������϶�̬��������  

# Դ�������ص�ַ��
<blockquote cite='�º���'>
GITHUBԴ�����ص�ַ:<strong>��<a href='https://github.com/chenhaoxiang/SpringBoot/tree/master/20171224/code/hello' target='_blank'>���ҽ�������</a>��</strong>
</blockquote>