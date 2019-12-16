## 构建spring项目详细步骤

准备工作：

- IDEA编辑器
- JDK1.8
- MAVEN3.6.0

#### 创建maven项目，并使用archtype脚手架构建项目

在properties那里为了**提高下载速度**，可以添加一个配置项

- archetypeCatalog：internal

####  补全项目结构

![image-20191214221251940](F:\我的博客\image\image-20191214221251940.png)

在src目录下需要添加

- java目录并标记为源代码根目录，用于编写class文件
- resources目录  并标记为resources，用于添加配置文件

#### 使用maven安装依赖

使用到的坐标如下：

```xml
<!--pom.xml-->
 <properties>
    <!--锁定spring的版本-->
    <spring.version>5.2.2.RELEASE</spring.version>
 </properties>

<!--添加spring-mvc相关依赖-->
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.3.3</version>
    </dependency>
  </dependencies>
```

#### 配置web.xml

配置servlet的相关配置节

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>spring-mvc小demo</display-name>
  <servlet>
    <servlet-name>app</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>app</servlet-name>
    <!--拦截所有的请求-->
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>

```

#### 配置spring-mvc配置文件

在resources文件夹下创建一个spring config文件，名称为`applicationContext/xml`

开启注解扫描，获取使用注解标记的bean对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans >
        <!--开启注解扫描-->
        <context:component-scan base-package="com.hhb"/>
</beans>
```

#### 加载spring-mvc配置文件

在web.xml添加如下配置，在spring-mvc程序启动时自动创建spring容器

```xml
<servlet>
    <servlet-name>app</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:applicationContext.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
```

#### 配置视图解析器

修改`applicationContext/xml`

```xml
<!--配置视图解析器-->
        <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/WEB-INF/pages"/>
            <property name="suffix" value=".jsp"/>
        </bean>
```

#### 最后完整的配置文件

web.xml

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>spring-mvc小demo</display-name>
  <servlet>
    <servlet-name>app</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <!--读取spring-mvc配置文件-->
      <param-value>classpath:applicationContext.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>app</servlet-name>
    <!--拦截所有的请求-->
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>

```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="
         http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/mvc
         http://www.springframework.org/schema/mvc/spring-mvc.xsd">
        <!--开启注解扫描-->
        <context:component-scan base-package="com.hhb" />
        <!--配置视图解析器-->
        <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/WEB-INF/pages/" />
            <property name="suffix" value=".jsp" />
        </bean>
        <!--开启spring-mvc框架注解-->
        <mvc:annotation-driven />
</beans>
```

