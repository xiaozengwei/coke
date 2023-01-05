<p align="center">
	<a href="https://gitee.com/needcoke/coke"><img src="./image/logo.png" width="45%"></a>
</p>
<p align="center">
    <strong style="">coke : a java ioc foundation framework </strong>
</p>

-------------------------------------------------------------------------------

## 📚简介
coke是一个类springboot的ioc框架，与之不同的是coke更为简单，纯粹。在使用方式上coke继承了springboot的简单易用，在框架本身上对开发者更加友好，是真正拥抱自定义框架的开发者的。

coke仅仅102KB大小，小型项目。由于其在预加载，容器后置处理上采用异步方案，使其相较于springboot项目拥有更快的启动速度。

coke拥有丰富的官方扩展包，[**🌎coke-extend项目**](https://gitee.com/needcoke/coke-extend)

-------------------------------------------------------------------------------

### 🎁coke适合哪些项目

<p>1、对框架国产化有要求的项目</p>
<p>2、对包体积有要求的场景</p>
<p>3、高启动速度、低web延迟项目</p>
<p>4、轻量级项目或demo</p>
<p>5、java脚本</p>

-------------------------------------------------------------------------------

## 📦安装

### 🍊Maven
在项目的pom.xml的dependencies中加入以下内容:

```xml
        <dependency>
            <groupId>org.needcoke</groupId>
            <artifactId>coke</artifactId>
            <version>1.0.2</version>
        </dependency>
```

### 🍐Gradle

```
implementation 'org.needcoke:coke:1.0.2'
```

-------------------------------------------------------------------------------

## 📦使用文档

### 1、启动类
[**🌎示例项目**](https://gitee.com/needcoke/coke-example/blob/master/example_run_demo/src/main/java/com/hello/coke/RunApplication.java)
使用coke框架必须要在启动类中按照如下格式书写,以RunApplication类为例:

```java
public class RunApplication {

    public static void main(String[] args) {
        CokeApplication.run(RunApplication.class, args);
    }
}
```

-------------------------------------------------------------------------------

### 2、常用注解

-------------------------------------------------------------------------------
#### 2.1 @Component

<p>a、@Component注解用于定义一个组件bean,该注解作用在类上。</p>
<p> b、coke启动时会为@Component类创建一个单例bean。 </p>
<p>c、通过@Component的value属性或者name属性可以修改bean在容器中的名称(唯一标识)，name属性的优先级高于value。</p>
如果不为name和value赋值，则coke会为该bean赋默认名称，类名首字母首字母小写，该场景下(coke自动分配)如果名称与其他bean重复则会加上@符加六位随机字符。手动分配场景出现名称重复会直接报错。

<p>参考如下:</p>

```java
import pers.warren.ioc.annotation.Component;

//@Component(name = "userService")
//@Component("userService")
@Component
public class UserService {
    
    public void sayHello(){
        System.out.println("hello coke");
    }
}

```

-------------------------------------------------------------------------------

#### 2.2 @Configuration
<p>a、@Configuration注解定义一个配置bean,该注解作用在类上。</p>
<p> b、coke启动时会为@Configuration类创建一个单例bean。 </p>
<p>c、通过@Configuration的value属性或者name属性可以修改bean在容器中的名称(唯一标识)，name属性的优先级高于value。</p>
如果不为name和value赋值，则coke会为该bean赋默认名称，类名首字母首字母小写，该场景下(coke自动分配)如果名称与其他bean重复则会加上@符加六位随机字符。手动分配场景出现名称重复会直接报错。

<p>
    <a style="color: crimson">注:@Configuration所生成的bean在生成顺序上理论优先于@Component</a>
</p>

-------------------------------------------------------------------------------

#### 2.3 @Value
@Value注解可以在bean中注入配置文件的配置。coke支持注入的配置文件包括: application.yml,application.yaml,application.properties,优先级从高到低。

<p>配置文件示例:</p>

```yaml
properties:
  demo: hello coke
  demo2: 233
```
<p>注入配置文件示例:</p>

```java
import pers.warren.ioc.annotation.Configuration;
import pers.warren.ioc.annotation.Value;

@Configuration
public class CokeConfig {

    @Value("properties.demo")
    private String demo;

    @Value("properties.demo2")
    private String demo2;

    @Value("properties.demo3:666")        //如果配置文件中找不到 properties.demo3对应的值，则赋予该字段默认值 666
    private String demo3;
}
```

#### 2.4 @Bean

<p> a、@Bean用于定义一个简单bean，作用在方法上(该方法必须是@Component或@Configuration标注的类中的方法)</p>
<p> b、coke启动时会为@Bean类创建一个单例bean。 </p>
<p> c、生成的bean的名称默认会使用@bean所标注的方法名,也可以通过@Bean的name属性手动分配。 </p>
coke自动分配名称，如果名称与其他bean重复则会加上@符加六位随机字符。手动分配场景出现名称重复会直接报错。

<p>示例如下:</p>

```java
import com.hello.coke.cp.User;
import pers.warren.ioc.annotation.Bean;
import pers.warren.ioc.annotation.Configuration;

@Configuration
public class CokeConfig {

    @Bean(name = "userDemo2")
    public User userDemo(){
        return new User();
    }
}
```