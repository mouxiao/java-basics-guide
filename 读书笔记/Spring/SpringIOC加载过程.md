### Spring IOC加载过程
> IOC：控制反转，管理bean，


#### 扫描
##### 基于XML配置[ClassPathXmlApplicationContext]
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  (1) (2)
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```
##### 基于文件[FileSystemXmlApplicationContext]
##### 基于注解[AnnotationConfigApplicationContext]
> @Configuration, @Bean, @Service 等