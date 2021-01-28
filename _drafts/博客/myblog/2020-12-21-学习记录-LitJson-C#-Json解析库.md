---
layout: post
title: LitJson - json解析库
date: 2020-12-21 11:34:33
categories: Json
tag: C#基础
---





#### class JsonData

JsonData是数据基础类。一个JSON数据在LitJSON中即表示为JsonData。
JsonData有多种类型，分别为：

```json
JsonType.Array
JsonType.Boolean
JsonType.Double
JsonType.Int
JsonType.Long
JsonType.None
JsonType.Object
JsonType.String
```



顾名思义，这些类型就不用多做解释了。
若要创建一个JSON数据，可这样做：

```
JsonData json = new JsonData();
json.SetJsonType(JsonType.Object);
json["name"] = "CYF";
json["age"] = 30;
```



此代码创建的JSON数据用JSON格式表示为：

```
{"name":"CYF", "age":30}
```



读取JsonData中的数据：

```
String name = (String)json["name"]; // CYF
int age = (int)json["age"]; // 30
```



本人推荐使用此种方法生成JSON数据。虽然LitJSON提供了将实体类直接转换为JSON的方法，但本人并不建议使用。原因就不在此细说了，本文旨在简明地说明LitJSON的使用方法。
JsonData本质上是一个字典，保障了读写节点的性能。

#### method JsonData.ToJson()

ToJson()方法用来将JsonData数据转换为JSON格式的字符串。

```
String strJson = json.ToJson(); // {"name":"CYF", "age":30}
```



ToJson()另有两个重载方法，这里重点说说ToJson(bool)。
LitJSON出于性能的考量，对ToJson()的结果进行了缓存，当修改了一级节点的数据时，缓存会自动失效，但当修改的是二级节点及以后的节点，而没有明确地修改其对应的一级节点，那么此缓存将不会失效，当再次调用ToJson()方法时，得到的将是一个错误的结果。此时应调用ToJson(true)，明确表明不使用缓存的数据。

```
String strJson = json.ToJson(true);
```



#### method JsonMapper.ToObject(String)

此方法能将JSON格式的字符串转换为LitJSON中的JsonData。

```
String strJson = "{'name':'CYF', 'age':30}";
JsonData json = JsonMapper.ToObject(strJson);
```



#### JsonType.Array

在LitJSON中，JSON的任何数据类型——无论是String、Number、Object或者Array，都是JsonData。不同的数据类型用JsonType区分。
若要创建JSON数组，可这样做：

```
JsonData json = new JsonData();
json.SetJsonType(JsonType.Array);
json.Add(0);
json.Add(1);
json.Add(2);
String strJson = json.ToJson(); // [0, 1, 2]
```



遍历数组：

```
foreach (JsonData item in json)
{

int num = (int)item;

}
```



获得数组拥有的元素的数量：

```
int n = json.Count; // 3
```



#### method JsonData.ContainsKey(String)

此方法可判断JsonData中是否包含指定的节点。

```
if (json.ContainsKey("age"))
{

// do something

}
```



此方法仅对Object有效。（Array也不可能有key呀^_^）

#### class JsonWriter

最原始、最直接地生成JSON格式字符串的方法是使用JsonWriter。

```
StringBuilder sb = new StringBuilder();
JsonWriter writer = new JsonWriter(sb);
writer.WriteObjectStart();
writer.WritePropertyName("name");
writer.Write("CYF");
writer.WritePropertyName("age");
writer.Write(30);
writer.WritePropertyName("items");
writer.WriteArrayStart();
writer.Write(0);
writer.Write(1);
writer.WriteArrayEnd();
writer.WriteObjectEnd();
String strJson = sb.ToString(); // {"name":"CYF", "age":30, "items":[0, 1]}
```





---

---


## 个人信息：
### 姓名：邹建

### 19年加入 四川省装备制造机器人应用技术工程实验室，Dream Studio 软件工作室，负责软件开发相关工作
### 擅长：Unity游戏开发，Unity机器人仿真，C#编程语言，工业软件开发

### QQ：451991189
### B站ID：UnitySir
### 个人博客：
### [UnitySir - github.io](https://unitysir.github.io/)
### [UnitySir - 博客园 (cnblogs.com)](https://www.cnblogs.com/unitysir/)
### [- UnitySir (gitee.io)](https://unitysir.gitee.io/)
### [UnitySir (bilibili)](https://space.bilibili.com/308511666)
### B站ID：UnitySir

## 如果内容对你有所帮助：
<img src="https://pic4.zhimg.com/v2-87fbc8ee6ab3fd92f423d414d039b627_b.jpeg" width="300px"/>
<img src="https://pic2.zhimg.com/v2-b8ab4acf7899b2ced11287cdbd8279b5_b.jpeg" width="300px"/>

---