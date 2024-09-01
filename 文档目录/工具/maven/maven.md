# pom.xml文件详细解读

## 一、什么是pom
pom(Project Object Model)，即项目对象模型，用以描述maven项目的各种信息。

## 二、pom配置概览
pom.xml配置文件主要由几个部分组成：基本配置、父POM配置、项目信息、maven坐标、打包方式、子模块列表、属性列表、依赖配置、依赖管理、构建管理、多环境配置管理
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- =====父POM配置===== -->
    <parent>
        <groupId>...</groupId>
        <artifactId>...</artifactId>
        <version>...</version>
        <relativePath>...</relativePath>
    </parent>

    <!-- =====项目信息===== -->
    <!--项目名-->
    <name>...</name>
    <!--项目描述-->
    <description>...</description>
    <!--项目url-->
    <url>https://...</url>
    
    <!-- =====maven坐标===== -->
    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <version>...</version>
    
    <!-- =====打包方式jar/war===== -->
    <packaging>...</packaging> 
    
    <!-- =====子模块列表===== -->
    <modules>...</modules>

    <!-- =====属性列表===== -->
    <properties>...</properties>
    
    <!-- =====依赖配置===== -->
    <dependencies>...</dependencies>
    
    <!-- =====依赖管理===== -->
    <dependencyManagement>...</dependencyManagement>
    
    <!-- =====构建管理===== -->
    <build>...</build>
    
    <!-- =====多环境配置管理===== -->
    <profiles>...</profiles>
</project>
```
## 三、基本配置
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
</project>
```
本部分为pom.xml的文件头，起到声明作用，一般固定不变，需要注意的是：modelVersion指定pom.xml符合哪个版本的描述符，maven2和3只能为4.0.0。

## 四、父POM配置
```xml
<parent>
    <groupId>com.craboss</groupId>
    <artifactId>ParentModule</artifactId>
    <version>1.0-SNAPSHOT</version>
    <relativePath>../pom.xml</relativePath>
</parent>
```
本配置主要用于多模块管理项目中的子模块配置，在多模块项目中会定义一个父模块统一管辖多个子模块，用来统一依赖版本或者一些配置项。

relativePath标签用于指定子模块与其父模块 POM 文件的相对路径。它主要用于帮助 Maven 定位父 POM 文件，以便子模块可以正确继承父 POM 中的配置。
如果 relativePath 没有显式指定，Maven 默认会寻找 ../pom.xml 作为父 POM 文件（即子模块所在目录的上一级目录中的 pom.xml 文件）

## 五、项目信息
```xml
  <!--项目名-->
  <name>maven-notes</name>
  <!--项目描述-->
  <description>maven 学习笔记</description>
  <!--项目url-->
  <url>https://github.com/dunwu/maven-notes</url>
  <!--项目开发年份-->
  <inceptionYear>2024</inceptionYear>
  <!--开源协议-->
  <licenses>
    <license>
      <name>Apache License, Version 2.0</name>
      <url>https://www.apache.org/licenses/LICENSE-2.0.txt</url>
      <distribution>repo</distribution>
      <comments>A business-friendly OSS license</comments>
    </license>
  </licenses>
  <!--组织信息(如公司、开源组织等)-->
  <organization>
    <name>...</name>
    <url>...</url>
  </organization>
  <!--开发者列表-->
  <developers>
    <developer>
      <id>victor</id>
      <name>craboss</name>
      <email>craboss@163.com</email>
      <url>https://github.com/craboss</url>
      <organization>...</organization>
      <organizationUrl>...</organizationUrl>
      <roles>
        <role>architect</role>
        <role>developer</role>
      </roles>
      <timezone>+8</timezone>
      <properties>...</properties>
    </developer>
  </developers>
  <!--代码贡献者列表-->
   <contributors>
    <contributor>
      <!--标签内容和<developer>相同-->
    </contributor>
  </contributors>
```
项目信息相关的这部分标签都不是必要的，也就是说完全可以不填写，它的作用仅限于描述项目的详细信息。

## 六、maven坐标
```xml
    <groupId>com.craboss</groupId>
    <artifactId>ChildModule</artifactId>
    <version>1.0-SNAPSHOT</version>
```
在maven中，根据groupId:artifactId:version来唯一识别一个jar包。

## 七、打包方式
```xml
<packaging>war</packaging>
```
打包方式主要有三种：jar、war以及pom。

对于多模块管理项目中，父模块的打包方式必须是pom，用来管理子模块。项目打jar包或者打war包，其生成的压缩文件.jar或者.war的包结构以及用处都不一致。

**jar包：** JAR包是标准的Java应用程序打包格式，通常包含应用程序的所有类文件、依赖库和资源文件。JAR 包主要用于非Web应用程序，但在Spring Boot项目中，它可以包含嵌入式Web服务器（如 Tomcat），使得 JAR 包也能用于 Web 应用程序。

打jar包时，也分三种情况：一种是非springboot可执行jar包，即普通胖jar，其包含应用程序的类文件和所有依赖的第三方库，是自包含的、可运行的 JAR 文件；第二种是springboot类型的可执行jar包，
由于springboot项目加载依赖的特殊性，所以需要将对应的类加载器也一并打入jar包中；第三种是普通jar包，只包含应用程序的类文件，不包含依赖的第三方库，在被其他包引用编译时，再从中央仓库或者本地等地方加载依赖的jar包。

1、普通jar包的包结构
```vbnet
ChildModuleOne-1.0-SNAPSHOT.jar
├── META-INF
│   ├── MANIFEST.MF
│   └── maven
│       └── com.craboss
│           └── ChildModuleOne
│               ├── pom.properties
│               └── pom.xml
├── com/
│   └── example/
│       └── MyApp.class
└── application.properties
```

2、非springboot项目可执行jar包结构

```vbnet
myapp.jar 
├── lib/                       // 存放依赖的 JAR 文件
│   ├── dependency1.jar
│   ├── dependency2.jar
│   └── ...
├── com/
│   └── example/
│       └── MyApp.class        // 应用程序的类文件
└── META-INF/
    └── MANIFEST.MF            // 清单文件，指定依赖的类路径
```
其在 MANIFEST.MF 中会指定依赖类路径，执行时指引类加载器去对应路径(Class-Path)进行加载:
```vbnet
Manifest-Version: 1.0
Mmain-Class: com.xxx.xxx.xxx.xxx
Built-By: xxx
Agent-Class: com.xxx.xxx.xxx.xxx
Can-Redefine-Classes: true
Can-Retransform-Classes: true
Class-Path: lib/byte-buddy-1.14.12.jar lib/byte-buddy-agent-1.14.12.ja
 r lib/lombok-1.18.26.jar lib/log4j-slf4j-impl-2.18.0-jdsec.rc2.jar li
 b/slf4j-api-1.7.25.jar lib/log4j-api-2.18.0-jdsec.rc2.jar lib/log4j-c
 ore-2.18.0-jdsec.rc2.jar lib/httpclient-4.5.3.jar lib/httpcore-4.4.6.
 jar lib/commons-logging-1.2.jar lib/commons-codec-1.9.jar lib/fastjso
 n-1.2.83-jdsec.rc1.jar lib/commons-lang3-3.9.jar lib/commons-collecti
 ons4-4.4.jar lib/mvel2-2.4.14.Final.jar lib/lite-3.1.15.jar lib/jsel-
 0.1.0.jar lib/spring-core-5.3.8.jar lib/spring-jcl-5.3.8.jar lib/jade
 -0.18.0.RC2.jar lib/jade-jdk6-0.18.0.RC2.jar lib/commons-io-2.5.jar l
 ib/commons-vfs2-2.1.jar lib/guava-19.0.jar lib/titan-profiler-sdk-202
 21231.1.jar lib/netty-resolver-dns-native-macos-4.1.89.Final.jar lib/
 netty-resolver-dns-classes-macos-4.1.89.Final.jar lib/netty-common-4.
 1.89.Final.jar lib/netty-resolver-dns-4.1.89.Final.jar lib/netty-buff
 er-4.1.89.Final.jar lib/netty-resolver-4.1.89.Final.jar lib/netty-tra
 nsport-4.1.89.Final.jar lib/netty-codec-4.1.89.Final.jar lib/netty-co
 dec-dns-4.1.89.Final.jar lib/netty-handler-4.1.89.Final.jar lib/netty
 -transport-native-unix-common-4.1.89.Final.jar lib/jsf-1.7.5-HOTFIX-T
 6.jar lib/javassist-3.19.0-GA.jar lib/netty-all-4.1.25.Final.jar lib/
 authsdk-common-1.0.RELEASE.jar lib/cfg-encryption-api-1.0.RELEASE.jar
  lib/authsdk-client-1.0.RELEASE.jar lib/audit-logger-api-1.0.RELEASE.
 jar lib/authsdk-server-1.0.RELEASE.jar lib/traceholder-1.0.1.jar lib/
 jsf-limiter-sdk-1.0.2-20221012.114925-8.jar lib/spring-context-5.3.8.
 jar lib/spring-aop-5.3.8.jar lib/spring-beans-5.3.8.jar lib/spring-ex
 pression-5.3.8.jar
Can-Set-Native-Method-Prefix: true
Created-By: Apache Maven 3.9.4
Build-Jdk: 1.8.0_381
```
3、springboot项目可执行jar包结构
```
myapp.jar
├── META-INF/                       // JAR 包的元数据目录
│   ├── MANIFEST.MF                 // 清单文件，包含启动类的信息等
│   └── other metadata files
├── BOOT-INF/                       // Spring Boot 特有的目录，包含应用和依赖
│   ├── classes/                    // 应用程序的类文件和资源文件
│   │   └── com/example/...         // 应用的包结构和 .class 文件
│   └──lib/                        // 所有第三方依赖的 JAR 文件
│      ├── spring-core-5.3.8.jar   // 示例依赖库
│      ├── spring-context-5.3.8.jar
│      ├── other-dependency.jar
│      └── ...
└── org/
    └── springframework/
        └── boot/
            ├── loader/             // Spring Boot 自定义类加载器相关的类
            │   ├── JarLauncher.class
            │   ├── LaunchedURLClassLoader.class
            │   ├── ...
            ├── JarLauncher.class   // 默认的启动器类，指定为 Main-Class
            └── other classes...
```
一个 可执行的 Spring Boot JAR 包的目录结构与普通的 JAR 包不同，主要因为它需要包含所有依赖的库和自定义的类加载器，
以实现自包含和独立运行的能力

**war包：** WAR 包是用于 Java Web 应用程序的归档文件，必须符合 Java EE 标准的 Web 应用目录结构。WAR 包不仅包含应用程序的类和依赖库，
还包含了特定的 Web 资源和配置文件。
```agsl
my-app.war
├── META-INF/
│   └── MANIFEST.MF
├── WEB-INF/
│   ├── web.xml
│   ├── classes/
│   │   └── com/
│   │       └── example/
│   │           └── MyApp.class
│   ├── lib/
│   │   └── (依赖库 *.jar)
│   └── spring-web.xml
├── index.jsp
└── static/
    ├── css/
    ├── js/
    └── images/
```
关键目录和文件
- META-INF/MANIFEST.MF：与 JAR 包类似，包含包的元数据。
- WEB-INF/：这是 WAR 包特有的目录，用于存放 Web 应用的私有文件，浏览器无法直接访问这个目录。
- WEB-INF/web.xml：Web 应用程序的部署描述符文件，用于配置 Servlet、过滤器、监听器等。对于 Spring Boot 项目，这个文件可以省略。
- WEB-INF/classes/：包含编译后的 .class 文件，这个目录相当于 JAR 包中的根目录。
- WEB-INF/lib/：包含该 Web 应用程序所依赖的外部 JAR 包。
- WEB-INF/spring-web.xml：Spring 应用程序的配置文件（可选）。
- index.jsp：典型的 JSP 页面，作为应用的入口页面。
- static/：存放静态资源文件，如 CSS、JavaScript 和图片等。

war包无需添加对应类加载器，因为其运行在tomcat上，tomcat中已经包含了一套完整的类加载机制用来加载各种依赖。

## 八、子模块列表
```agsl
  <modules>
    <module>ChildModuleOne</module>
    <module>ChildModuleTwo</module>
    <module>ChildModuleThree</module>
    <module>ChildModuleFour</module>
  </modules>
```
子模块列表主要存在于父模块中，用来使得父模块统一管理其下子模块。

## 九、属性列表
```agsl
  <properties>
    <maven.compiler.source>1.7<maven.compiler.source>
    <maven.compiler.target>1.7<maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
  </properties>
```
定义的属性可以在 pom.xml 文件中任意处使用(包括子模块的pom)，使用方式为 ${propertie} 。

注意，在profiles的profile中，也可以为多环境分别配置properties数据，一般用来为不同环境订制不同的配置信息，如jar包版本等等。












