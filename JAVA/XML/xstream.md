#### XStream 依赖

```
 <dependency>
 	<groupId>com.thoughtworks.xstream</groupId>
 	<artifactId>xstream</artifactId>
 	<version>1.4.9</version>
 </dependency>
```

#### XML转对象

```
 public static Object convertFromXML(Class<?> clazz, String xml) {
        XStream xStream = new XStream(new DomDriver());
        // 避免 com.thoughtworks.xstream.security.ForbiddenClassException异常
        xStream.addPermission(AnyTypePermission.ANY);
        // 处理注解
        xStream.processAnnotations(clazz);
        // 将XML字符串转为bean对象
        return xStream.fromXML(xml);
    }
```

#### 对象转XML

```
  public static String convertToXml(Object obj) {
        XStream xs = new XStream(new DomDriver());
        xs.processAnnotations(obj.getClass());
        return XML_HEAD + xs.toXML(obj);
 }
```

#### 注解详解

```
@XStreamAlias("xx") //xml节点别名
@XStreamImplicit(itemFieldName = "xx") //列表
@XStreamAsAttribute //取xml属性
```

