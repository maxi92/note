# SpringBoot @ConfigurationProperties注解的使用

[TOC]

## 注意

1.@ConfigurationProperties的prefix值代表了在配置文件中可以通过这个前缀直接设置Bean中属性的值。比如下文中prefix为"person"，则在配置文件中可以用person.last-name=zhangsan来设置属性值。

2.注意区分@ConfigurationProperties注解和@Value注解的区别

## @ConfigurationProperties给属性映射值

编写JavaBean

~~~java
/*

- 将配置文件application.properties中配置的每一个属性值映射到当前类的属性中；
- @ConfigurationProperties：告诉springboot将本类中所有属性和配置文件中相关的配置进行绑定；
- prefix="person"：指出将配置文件中person下的所有属性进行一一映射；
  *
- 注意：只有当前这个类是容器中的组件时，才可以用容器提供的@ConfigurationProperties功能；
  *
- */

@Component
@ConfigurationProperties(prefix="person")
public class Person {

```
private String lastName;
private Integer age;
private Boolean boss;
private Date birth;
 
private Map<String,Object> maps;
private List<Object> lists;
 
@Override
public String toString() {
    return "Person{" +
            "lastName='" + lastName + '\'' +
            ", age=" + age +
            ", boss=" + boss +
            ", birth=" + birth +
            ", maps=" + maps +
            ", lists=" + lists +
            '}';
}
```

   getter...
   setter...

}
~~~

编写配置文件

```java
#private String lastName;
#private Integer age;
#private Boolean boss;
#private Date birth;
#
#private Map<String,Object> maps;
#private List<Object> lists;

#配置person的属性值
person.last-name=zhangsan
person.age=18
person.boss=false
person.birth=1992/02/20
person.maps.k1=v1
person.maps.k2=111
person.lists=a,b,c
```

测试类：

~~~java
@RunWith(SpringRunner.class)
@SpringBootTest
public class TestApplicationConfig {
    @Autowired
    Person person;

```
@Test
public void testPersonProperties(){
    System.out.println(person);
}
```

}
~~~

运行结果：

![img](https://img-blog.csdn.net/20180808225312905?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1hVOTA2NzIy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## @Value给属性设置值

编写JavaBean

```
@Component
//@ConfigurationProperties(prefix="person")
public class Person {

​```
@Value("${person.last-name}") // 从配置文件中获取值
private String lastName;
 
@Value("#{2*8}")  // spring表达式
private Integer age;
 
@Value("true") // boolean值
private Boolean boss;
private Date birth;
 
private Map<String,Object> maps;
private List<Object> lists;
 
@Override
public String toString() {
    return "Person{" +
            "lastName='" + lastName + '\'' +
            ", age=" + age +
            ", boss=" + boss +
            ", birth=" + birth +
            ", maps=" + maps +
            ", lists=" + lists +
            '}';
}
​```

}
```

运行结果：

![img](https://img-blog.csdn.net/20180808231409860?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1hVOTA2NzIy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

运行结果：

## @ConfigurationProperties和@Value对比

 	![1556501264849](C:\Users\xima\AppData\Roaming\Typora\typora-user-images\1556501264849.png)

如果一个JavaBean中大量属性值要和配置文件进行映射，可以使用@ConfigurationProperties；