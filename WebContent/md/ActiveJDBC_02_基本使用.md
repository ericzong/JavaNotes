# 编写模型

ActiveJDBC 的模型类必须派生自 org.javalite.activejdbc.Model。

## 映射

### 映射推断

模型类可以不指定映射的表，ActiveJDBC 会根据英文单复数的变形来推断映射关系。

简单说来就是，单数形式的模型名映射到对应复数形式的表，比如：Computer => computers。

但这种变形并不仅限于在单数形式后加“s”转变为复数形式的情况，也包括一些特殊的变形方式。详细可参考官方对于 [English inflections](http://javalite.io/english_inflections) 的说明。

### 覆盖映射

自动的映射推断很方便，但限制了表与模型的命名，某些情况下，这可能不是我们想要的。

针对这种情况，ActiveJDBC 提供了 @Table 注解来覆盖指定模型映射的表。

## setter/getter

ActiveJDBC 不通过属性映射表字段，因此，也不需要编写 setter/getter 方法，而是使用 Model.get(fieldName) 和 Model.set(fieldName, value) 替代它们，并且 Model 还提供了许多重载或是绑定不同数据类型的便捷方法。

get() 方法甚至会将 CLOB 字段自动转换为字符串。

# 常用 API

```java
model.saveIt();
model.findFirst(where, params);
model.where(where, params);
model.delete();
Model.deleteAll();
Model.findAll();
```



