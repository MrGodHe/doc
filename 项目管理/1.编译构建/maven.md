**打印依赖树**

```
mvn dependency:tree
```

**编译打包**

```
mvn clean package -Dmaven.test.skip=true
#指定配置文件
mvn clean package -Dmaven.test.skip=true -s settings-xx.xml
```

**编译部署本地包**

```
mvn clean install -Dmaven.test.skip=true -s settings-xx.xml

#命令部署
#(1)指定GAVP并自动生成最小化的pom.xml
mvn install:install-file -DgroupId=  -DartifactId=  -Dversion=  -Dpackaging=jar -Dfile=/xxx/xxx.jar
#(2)手动指定pom.xml
mvn install:install-file -Dfile=/xxx/xx.jar -DpomFile=/xx/pom.xml
#(3)使用JAR包内的pom.xml
mvn org.apache.maven.plugins:maven-install-plugin:3.0.0-M1:install-file -Dfile=brCommonApi-1.0.0.jar
```

**部署到私服**

```
mvn deploy -s settings-xx.xml

#命令部署
mvn deploy:deploy-file
-DgroupId=
-DartifactId=
-Dversion=
-Dpackaging=jar
-Dfile=/xxx/xxx.jar
-Durl=http://xxx.xxx.xxx.xxx:8081/repository/maven-snapshots/
-DrepositoryId=snapshots
```

**跳过测试：**

```
#不执行测试用例，也不编译测试用例类。
-Dmaven.test.skip=true

#跳过单元测试，但是会继续编译。
-DskipTests=true 
```



 **打包插件**

- maven-jar-plugin，默认的打包插件，用来打普通的project JAR包；
- maven-shade-plugin，用来打可执行JAR包，也就是所谓的fat JAR包；
- maven-assembly-plugin，支持自定义的打包结构，也可以定制依赖项等。

