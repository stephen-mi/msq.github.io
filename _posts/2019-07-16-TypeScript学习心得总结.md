---
layout:     post
title:      TypeScript学习心得总结（一）
subtitle:   TypeScript基础
date:       2019-07-16
author:     MSQ
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - TypeScript
---


### 为什么使用TypeScript

- JavaScript的超集

支持所有原生JavaScript的语法

- 强类型语言

现在很多主流语言都是强类型的，而这点也一直是JavaScript所被人诟病的地方。使用TypeScript之后，将会在代码调试、重构等步骤节省很多时间。
比如说：函数在返回值的时候可能经过复杂的操作，那我们如果想要知道这个值的结构就需要去仔细阅读这段代码。那如果有了TypeScript之后，直接就可以看到函数的返回值结构，将会非常的方便。

-强大的IDE支持

现在的主流编辑器如VSCode、WebStorm、Atom、Sublime等都对TypeScript有着非常友好的支持，主要体现在智能提示上，非常的方便。

-迭代更新快

-微软和Google支持

TypeScript是微软开发的语言，而Google的Angular使用的就是TypeScript，所以不用担心会停止维护，至少在近几年内TypeScript都会是一门主流开发语言。

### 5分钟上手TypeScript

#### 安装TypeScript
针对使用npm的用户：
```
>npm install -g typescript
```
#### 构建你的第一个TypeScript文件
新建一个demo.ts文件:
```
function greeter(person: string) {
    return "Hello, " + person;
}

let user = "World";

document.body.innerHTML = greeter(user);
```
#### 编译代码
```
tsc demo.ts
```
编译成功后，会在当前目录下生成一个demo.js文件：
```
function greeter(person) {
    return "Hello, " + person;
}
var user = "World";
document.body.innerHTML = greeter(user);
```
新建一个demo.html：
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>

</body>
<script src="classDemo.js"></script>
</html>
```
测试结果：
```
Hello, World
```
#### 接口
```
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```
#### 类
```
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);

待更新...
以上都是个人总结出来的，肯定也有不对的地方，欢迎交流指点，互相学习。
