# 编写模型

ActiveJDBC 的模型类必须派生自 org.javalite.activejdbc.Model。

## 模型与表的映射

### 映射推断

模型类可以不指定映射的表，ActiveJDBC 会根据英文单复数的变形来推断映射关系。

简单说来就是，单数形式的模型名映射到对应复数形式的表，比如：Computer => computers。

但这种变形并不仅限于在单数形式后加“s”转变为复数形式的情况，也包括一些特殊的变形方式。详细可参考官方对于 [English inflections](http://javalite.io/english_inflections) 的说明。

### 覆盖映射

自动的映射推断很方便，但限制了表与模型的命名，某些情况下，这可能不是我们想要的。

针对这种情况，ActiveJDBC 提供了 @Table 注解来覆盖指定模型映射的表。

## 模型与数据库关联

默认情况下，模型与“default”数据库关联，但可使用 @DbName 注释覆盖。

## setter/getter

ActiveJDBC 不通过属性映射表字段，因此，也不需要编写 setter/getter 方法，而是使用 Model.get(fieldName) 和 Model.set(fieldName, value) 替代它们，并且 Model 还提供了许多重载或是绑定不同数据类型的便捷方法。

> 关于 Clob
>
> set() 方法可以将字符串写入 Clob 字段中。
> get() 方法可以从 Clob 字段中读取数据为字符串或强转为 Clob 类型进行处理，而 getString() 方法可将 Clob 字段读取为字符串。
>
> 对于缓存的模型而言，Clob 字段将被事先读取入字符串中，即使连接断开，事后亦可访问。但可能会有较大的内存占用。

get() 方法甚至会将 CLOB 字段自动转换为字符串。

## 主键

### 复合键

使用 @CompositePK 注解复合键，并使用 Model.findByCompositeKeys() 查询（确保参数顺序与注解一致）。

### 覆盖主键

使用 @IdName 注解覆盖主键。

### 主键生成器

使用 @IdGenerator 注解主键生成器。

## 管理时间

如果表中存在 created_at 和 updated_at 列，并且其类型对应 java.sql.Timestamp，那么，框架将自动管理这些时间列。除非使用 model.manageTime(false); 禁用。

# 数据库管理

数据库连接通过 Base 和 DB 类附加到一个本地线程 Map 变量中，Base 总是操作名为“default”的数据库连接。

DB 可以使用 TWR 自动关闭。

不能直接使用连接池，只能通过 DB 打开一个数据源来使用。

## 属性文件配置

属性文件名为 database.properties，放置于 classpath 根目录。

## 环境变量

配置属性前缀用于 ACTIVE_ENV 环境变量中，以指定无参数时，open() 方法所使用的数据库参数。如果该环境变量未配置，默认值为 development。

## 环境变量覆盖

```
ACTIVEJDBC.URL
ACTIVEJDBC.USER
ACTIVEJDBC.PASSWORD
ACTIVEJDBC.DRIVER

ACTIVEJDBC.JNDI
```

注：覆盖属性文件配置。

## 系统变量覆盖

```
activejdbc.url
activejdbc.user
activejdbc.password
activejdbc.driver

activejdbc.jndi
```

注：覆盖属性文件和环境变量配置。

## 指定 schema

```
-Dactivejdbc.default.schema=myschema
```

# 模型常用 API

```java
model.set(name, value)
model.saveIt();
model.findFirst(where, params);
model.where(where, params);
model.delete();
Model.deleteAll();
Model.findAll();
Model.count();
Model.count(query, params);
Model.findBySQL(query, params);
```

## 创建保存

### save() vs. saveIt()

save() 和 saveIt() 在保存之前都会进行校验，区别在于校验失败时，后者抛出异常，而前者不会抛出异常，而校验失败的信息可通过 errors() 方法获取。

### create() vs. createIt()

跟 save() 和 saveIt() 类似。

## 查询

```java
Model.where("columnName = 'value'")
```

### 参数化

```java
Model.where("columnName = ?", "value")
```

### 处理大结果集

```java
Model.findWith(final ModelListener listener, String query, Object ... params)
```

### Dump

```java
Model.findAll().dump();
```

### 限制和排序

```java
lazyList.limit(limit).offset(offset).orderBy(orderBy);
```

### 分页

```java
new Paginator(modelClass, pageSize, query, params).orderBy(orderBy);
paginator.getPage(pageNumber);
paginator.getCurrentPage();
paginator.pageCount();
```

### 延迟加载

默认 ActiveJDBC 查询是延迟加载的。

可急切加载一对多、多对一、多对多的上下级关系：

```java
lazyList.include(ParentOrChildClass);
lazyList.toMaps();
```

## 批处理

```java
Model.updateAll("columnName = ?", params);
Model.update("columnName = ?", "columnName2 = 'value'", params);
Model.deleteAll();
Model.delete(query, params);

PreparedStatement ps = Base.startBatch(stm);
Base.addBatch(ps, params);
...
Base.executeBatch(ps);
ps.close();
```

