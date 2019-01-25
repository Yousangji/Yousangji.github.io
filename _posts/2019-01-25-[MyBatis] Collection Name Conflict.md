---
title: "Mybatis Resolve Name Conflict"
date: 2019-01-25 18:26:28 -0400
layout: home
categories:
  - Spring
---

####Nested Collection 
Nested Class could have some fields named identical as parent class.

```java
public class Parent{
    int id;
    String email;
    List<Child> childList;
   }
   
public class Child{
    int id;
    String email;
    String name;
    }
```

####Collection
```
<resultMap id="Parent" type="Parent">
    <id property="id" column="id"/>
    <result property="email" column="email"/>
    <collection property="childList" resultMap="Child"/>
</resultMap>

<resulteMap id="Child" type="Child">
    <id property="id" column="id"/>
    <result property="email" column="email"/>
    <result property="name" column="name"/>
</resultMap>
```

~~~
<select id ="getParentById" parameterType="Integer" resultMap = "Parent" >
SELECT
parent.*, child.*
FROM parent
LEFT JOIN child on (child.parent_id = parent.id)
WHERE parent.id = #{parentId}
</select>
~~~

This code results wrong.
As Parent has a field name 'id','email', Child's id and email could be override with parent's.

Mybatis map sql results with resultMap.column, mapped value to class' property.
So, what should we do is *giving alias on fields name.*

```xml
<resultMap id="Parent" type="Parent">
    <id property="id" column="id"/>
    <result property="email" column="email"/>
    <collection property="childList" resultMap="Child"/>
</resultMap>

<resulteMap id="Child" type="Child">
    <id property="id" column="c_id"/>
    <result property="email" column="c_email"/>
    <result property="name" column="name"/>
</resultMap>
```


~~~
<select id ="getParentById" parameterType="Integer" resultMap = "Parent" >
SELECT
parent.*,
child.id as c_id,
child.email as c_email,
child.name
FROM parent
LEFT JOIN child on (child.parent_id = parent.id)
WHERE parent.id = #{parentId}
</select>
~~~

