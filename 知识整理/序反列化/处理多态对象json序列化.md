## 
jackson 的json序列化可以处理多态对象的序列化
需要添加的注解如下:
@JsonTypeInfo(use = JsonTypeInfo.Id.CLASS, property = "@class")