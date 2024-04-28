# 静态文档生成在线工具

gitbook的开源版本没怎么维护了，但是后面社区又有人接着维护，那就是honkit

https://github.com/honkit/honkit



```
$ npm init --yes
$ npm install honkit --save-dev

$ npx honkit init
$ npx honkit serve #OR $ npx honkit build
```

### 修改SUMMARY.md中的条目

比如最终内容如下

注意点：Introduction条目需要有，否则也会自动生成一个空的Introduction

```
# Summary

* [Introduction](README.md)
* [章节1](docs/章节1.md)
    * [章节1.1](docs/章节1.1.md)
* [章节2](docs/章节2.md)
```