title: jersey使用指南
tags:
    - 'jersey'
    - 'RESTful'
-----

**jersey** 是基于Java的一个轻量级RESTful风格的Web Services框架。

[官网](https://jersey.java.net/)

## 引入
使用maven，在pom.xml中加入：

```xml
<!-- Jersey -->
<dependency>
<groupId>org.glassfish.jersey.core</groupId>
    <artifactId>jersey-client</artifactId>
    <version>${jersey.version}</version>
</dependency>
<dependency>
    <groupId>org.glassfish.jersey.containers</groupId>
    <artifactId>jersey-container-servlet</artifactId>
    <version>${jersey.version}</version>
</dependency>
<dependency>
    <groupId>org.glassfish.jersey.media</groupId>
    <artifactId>jersey-media-moxy</artifactId>
    <version>${jersey.version}</version>
</dependency>
<dependency>
    <groupId>org.glassfish.jersey.media</groupId>
    <artifactId>jersey-media-multipart</artifactId>
    <version>${jersey.version}</version>
</dependency>
```
当然必不可少的，也需要使用`Java EE`的支持：

```xml
<!-- JAVA EE -->
<dependency>
    <groupId>javax</groupId>
    <artifactId>javaee-api</artifactId>
    <version>7.0</version>
    <scope>provided</scope>
</dependency>
```

**Jar**包详解：
> jersey-client 是jersey提供的客户端包，封装了一些客户端操作的类
> jersey-container-servlet 是jersey的核心，服务端必备包
> jersey-media-moxy 是定义了jersry支持的常用的数据格式，json，xml都包括其中
> jersey-media-multipart 是jersey的上传文件的支持

## 配置
**jersey** 的使用，必须要有一个全局的配置类，这个类需满足以下条件：

* @ApplicationPath 注解该类，并且在参数中指定相对路径
* 继承 `org.glassfish.jersey.server.ResourceConfig` 类
* 该类**构造方法**中设置jersey的配置，比如指定接口的包路径

如下：
```java
@ApplicationPath("/")
public class RESTServiceConfig extends ResourceConfig {

  public RESTServiceConfig() {
    packages("web.rest");
    register(MultiPartFeature.class);
  }
}
```

## GET
`GET`例子：

```java
@GET
@Path("/thing")
public String get() {
    return "thing";
}
```

## POST
`POST`例子：

```java
@POST
@Path("/add")
public Boolean add(@FormParam("name") String name) {
    // TODO save
    return true;
}
```

## Param
**jersey**中有几种常用的接收参数的注解：

* @PathParam 接收链接中参数，如"/xxx/{name}/",@PathParm("name")
* @QueryParam 接收链接中的普通参数，如"/xxx?name=ttt",@QueryParam("name")
* @FormParm 接收post提交中的表单参数
* @FormDataParm 上传文件接收文件参数

## json
开发中，json已经常用到无处不在了，jersey对json的支持很好。接收json，需要使用@Consumes，注解指定解压方式：
```
@Consumes(MediaType.APPLICATION_JSON)
```

返回json需要使用@Produces注解，指定压缩方式：
```
@Produces(MediaType.APPLICATION_JSON)
```

## 文件上传

示例:

```
  @POST
  @Path("import-excel")
  @Consumes(MediaType.MULTIPART_FORM_DATA)
  @Produces(MediaType.APPLICATION_JSON)
  public ImportResultBean importForExcel(@FormDataParam("file") String fileString,
                                         @FormDataParam("file") InputStream fis,
                                         @FormDataParam("file") FormDataContentDisposition fileDisposition) {
    // TODO
    return ;
  }
```

## 文件下载

文件下载需要将`Response`对象的压缩方式，指定为：

```
@Produces(MediaType.APPLICATION_OCTET_STREAM)
```