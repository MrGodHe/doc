打印依赖树

```
mvn dependency:tree
```

编译打包

```
mvn clean package -Dmaven.test.skip=true
```

编译部署本地包

```
mvn clean install -DMaven.test.skip=true -s settings-xx.xml
```

部署到私服

```
mvn deploy -s settings-xx.xml
```

#跳过测试：

```
-DskipTests=true
-Dmaven.test.skip=ture
-Dmaven.test.skip.exec=true
```



 打包插件

- maven-jar-plugin，默认的打包插件，用来打普通的project JAR包；
- maven-shade-plugin，用来打可执行JAR包，也就是所谓的fat JAR包；
- maven-assembly-plugin，支持自定义的打包结构，也可以定制依赖项等。



# maven scope 

当前包的依赖范围和依赖的传递性。常见的可选值有：compile, provided, runtime, test, system，import

1. **compile**  

   不申明值的情况下（默认值），标识依赖包需要参与当前项目的编译，以及后续的测试和运行。是一个较强的依赖，打包时通常需要包含进去。

2. **provided**  

   只会在项目编译、测试阶段起作用；可以认为目标容器中已经提供这个依赖，但是在写代码编译阶段会用到这个依赖。

   例如：servlet-api，运行项目时，容器已经提供，就不需要Maven重复地引入一遍了。

3. **runtime**

   在运行、测试有效，编译时代码无效。

   例如：JDBC驱动实现，项目代码编译只需要JDK提供的JDBC接口，只有在测试或运行项目时才需要实现上述接口的具体JDBC驱动

4. **test**

   只在测试时有效，例如：JUnit。

5. **system**

   在项目编译、测试阶段起作用，

   与provided的区别是：用system范围的依赖时必须通过systemPath元素显式地指定依赖文件的路径。由于此类依赖不是通过Maven仓库解析的，而且往往与本机系统绑定，可能造成构建的不可移植，因此应该谨慎使用

   ```
   <dependency>
   	<groupId>javax.sql</groupId>
    	<artifactId>jdbc-stdext</artifactId>
    	<version>2.0</version>
    	<scope>system</scope>
    	<systemPath>${java.home}/lib/rt.jar</systemPath>
   </dependency>
   ```

6. **import**

   只能在POM文件的`<dependencyManagement>`中使用，从而引入其他的pom文件中引入依赖

   如：在Spring boot 项目的POM文件中，我们可以通过在POM文件中继承 Spring-boot-starter-parent来引用Srping boot默认依赖的jar包

   ```
   <parent>
   	<groupId>org.springframework.boot</groupId>
   	<artifactId>spring-boot-starter-parent</artifactId>
       <version>2.3.12.RELEASE</version>
   </parent>
   ```

   但是，通过上面的parent继承的方法，只能继承一个 spring-boot-start-parent。实际开发中，用户很可能需要继承自己公司的标准parent配置，这个时候可以使用 scope=import 来实现多继承。代码如下：

   ```
   <dependencyManagement>
   	<dependencies>
       	<dependency>
           	<groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-dependencies</artifactId>
               <version>2.3.12.RELEASE</version>
               <type>pom</type>
               <scope>import</scope>
       	</dependency>
   	</dependencies>
   </dependencyManagement>
   ```

   

​		



