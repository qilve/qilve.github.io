---
layout: post
title:  "Javas实体映射之mapstrcut"
date:   2018-11-14 12:00:00
categories: java
tags: [java, mapstruct]
---

### mapstrcut简介
MapStruct是一个用于生成类型安全的bean映射类的Java注解处理器。  
使用时只需要定义映射器接口，声明需要映射的方法。它就会在编译过程中，生成该接口的实现，
对源对象和目标对象进行映射。  
<!-- more -->

### maven导入
```
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-jdk8</artifactId>
    <version>${org.mapstruct.version}</version>
</dependency>
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-processor</artifactId>
    <version>${org.mapstruct.version}</version>
</dependency>
```

### 使用示例
推荐参考 [mapStruct官方文档](http://mapstruct.org/documentation/dev/reference/html/)。

#### 同名属性映射
```java
@Mapper
public interface UserBeanMapper {

    UserBeanMapper INSTANCE = Mappers.getMapper(UserBeanMapper.class);

    UserVO doToVO(UserDO userDO);
    
    public static void main(String[] args) {
        // 即可实现do到vo同名属性的映射
        UserVO userVO = UserBeanMapper.INSTANCE.doToVO(userDO);
    }
}

```
#### 非同名属性映射
```java
@Mapper
public interface FishTankMapperWithDocument {

    @Mappings({
        @Mapping(target = "fish.kind", source = "fish.type"),
        @Mapping(target = "fish.name", expression = "java(\"Jaws\")"),
        @Mapping(target = "plant", ignore = true ),
        @Mapping(target = "ornament", ignore = true ),
        @Mapping(target = "material", ignore = true),
        @Mapping(target = "quality.document", source = "quality.report"),
        @Mapping(target = "quality.document.organisation.name", constant = "NoIdeaInc" )
    })
    FishTankWithNestedDocumentDto map( FishTank source );
}

```

