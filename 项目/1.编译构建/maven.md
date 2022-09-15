打印依赖树

mvn dependency:tree

编译部署本地包

mvn clean install -DMaven.test.skip=true -s E:\software\maven\apache-maven-3.6.2\conf\settings-mt.xml

部署到私服

mvn deploy -s E:\software\maven\apache-maven-3.6.2\conf\settings-mt.xml

#跳过测试：

-DskipTests=true

-Dmaven.test.skip=ture

-Dmaven.test.skip.exec=true











