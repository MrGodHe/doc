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





