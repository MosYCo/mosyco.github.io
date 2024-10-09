今天配置Nginx进行代理的时候发现一个小问题，css文件已经加载回来了，点开源代码也能看见，
但是并没有应用到Dom上。一开始怀疑是路径问题，因为路径上多了一个/，但直接强制引入后还是不行。

后续发现请求css文件的时候请求头中的ContentType是`text\plain`，而正确的应该是`text\css`

查询配置文档后了解到nginx加载文件时可以自动附加contentType，而有一个对应的映射文件mime.types
在默认配置中引入了此文件，但是在拷贝配置的时候把这块给覆盖掉了，导致最终加载css文件的contentType不对

参考Nginx部分配置如下
```
http {
    #  引入类型映射文件
    include       mime.types;
    #  添加默认ContentType
    default_type  application/octet-stream;
}
```