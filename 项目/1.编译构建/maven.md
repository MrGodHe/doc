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











