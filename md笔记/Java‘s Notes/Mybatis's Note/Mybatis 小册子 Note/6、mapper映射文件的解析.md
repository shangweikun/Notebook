#### resultMap的解析

总结：

1.   核心处理类 `XMLMapperBuilder`，统一进行mapper文件的解析；
2.   for循环处理多个 `ResultMap `标签；
3.   从`XNode`中，读取标签，并特殊处理 **构造器** 和 **鉴别器；**
4.   解析并处理普通标签，例如`<id>`,`<result>`；
5.   调用`MapperBuilderAssistant`的方法，生成ResultMap对象，将上述解析的内容存储到里面；
6.   在`parse`方法的最后，重新解析不完整的`ResultMap`。

