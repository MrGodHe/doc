**Optional**

```
Optional.ofNullable().orElse();
```



**stream**

```
  // 将Stream转换为Map
        Map<String, String> map = stream.collect(
            Collectors.toMap(
                // 使用奇数索引作为键
                key -> key,
                // 使用偶数索引作为值
                value -> value,
                // 处理冲突的情况，这里简单地选择第一个值
                (existingValue, newValue) -> existingValue
            )
        );
```

