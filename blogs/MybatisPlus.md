---
layout: page
permalink: /blogs/MybatisPlus/index.html
title: MybatisPlus
---

# MybatisPlus

## MybatisPlus的引入

### 引入MybatisPlus依赖，代替Mybatis依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.3.1</version>
</dependency>
```

### 定义Mapper接口并继承BaseMapper

```java
public interface UserMapper extends BaseMapper<User> { }
```

### 在实体类上添加注解声明表信息

**MyBatisPlus基于反射获取实体类信息作为数据库表信息**

1. 类名驼峰转下划线作为表名
2. 名为id的字段作为主键
3. 变量名驼峰转下划线作为表的字段名

**例子：**

- @TableName：用来指定表名
- @TableId：用来指定表中的主键字段信息
- @TableField：用来指定表中的普通字段信息

**IdType枚举**

- AUTO：数据库自增长
- INPUT：通过set方法自行输入
- ASSIGN_ID：分配 ID，接口IdentifierGenerator的方法nextId来生成id，默认实现类为DefaultIdentifierGenerator雪花算法

**使用@TableField的常见场景**

- 成员变量名与数据库字段名不一致
- 成员变量名以is开头，且是布尔值
- 成员变量名与数据库关键字冲突
- 成员变量不是数据库字段

```java
@TableName("tb_user")
public class User {
    @TableId(value="id",type=IdType.AUTO)
	private Long id;
    
	@TableField("username") 
	private String name;
    
    @TableField("is_married")
    private Boolean isMarried;
    
    @TableField("`order`")
    private Integer order;
    
    @TableField(exist = false)
    private String address;
}
```

### 在application.yml中根据需要添加配置

```yaml
mybatis-plus:
	type-aliases-package: com.itheima.mp.domain.po # 别名扫描包
	mapper-locations: "classpath*:/mapper/**/*.xml" # Mapper.xml文件地址，默认值  configuration:
	map-underscore-to-camel-case: true # 是否开启下划线和驼峰的映射
	cache-enabled: false # 是否开启二级缓存
  	global-config:
		db-config:
			id-type: assign_id # id为雪花算法生成
			update-strategy: not_null # 更新策略：只更新非空字段
```



## 核心功能

### 条件构造器

#### 用法

- QueryWrapper和LambdaQueryWrapper通常==用来构建select、delete、update的where条件部分==
- UpdateWrapper和LambdaUpdateWrapper通常==只有在set语句比较特殊才使用尽量使用==
- LambdaQueryWrapper和LambdaUpdateWrapper，避免硬编码，**即传字段函数过去**

#### 基于QueryWrapper的查询

例2-1：更新用户名为jack的用户的余额为2000

```java
/*UPDATE user 
	SET balance = 2000 
	WHERE (username = "jack")*/

@Test
void testUpdateByQueryWrapper() {
    //1.要更新的数据
    User user = new User();
    user.setBalance(2000);
    //2.更新的条件
    QueryWrapper<User> wrapper = new QueryWrapper<User>().eq("username","jack");
    //3.执行更新
    userMapper.update(user,wrapper);
}
```

#### 基于UpdateWrapper的更新

例2-2：更新id为1,2,4的用户的余额，扣200

```java
/*UPDATE user 
	SET balance = balance - 200 
    WHERE id in (1, 2, 4)
*/

@Test
void testUpdateByQueryWrapper() {
    List<Long> ids = List.of(1L,2L,4L);
    UpdateWrapper<User> wrapper = new UpdateWrapper<User>()
            .setSql("balance = balance - 200")
            .in("id", ids);
    userMapper.update(null,wrapper);
}
```

### 自定义SQL

**利用MyBatisPlus的Wrapper来构建复杂的Where条件，然后自己定义SQL语句中剩下的部分。**

例2-2改：

1. 基于Wrapper构建where条件

   ```java
   List<Long> ids = List.of(1L, 2L, 4L);
   int amount = 200;
   // 1.构建条件
   LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<User>().in(User::getId, ids);
   // 2.自定义SQL方法调用
   userMapper.updateBalanceByIds(wrapper, amount);
   ```

2. 在mapper方法参数中用Param注解声明wrapper变量名称，必须是ew

   ```java
   void updateBalanceByIds(@Param("ew") LambdaQueryWrapper<User> wrapper, @Param("amount") int amount);
   ```

3. 自定义SQL，并使用Wrapper条件

   ```xml
   <update id="updateBalanceByIds">
   	UPDATE tb_user SET balance = balance - #{amount} ${ew.customSqlSegment}
   </update>
   ```

### Service接口

#### 使用流程

1. 自定义Service接口继承IService接口

   ```java
   public interface IUserService extends IService<User> {}
   ```

2. 自定义Service实现类，实现自定义接口并继承ServiceImpl类

   ```java
   public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements IUserService { }
   ```


#### 案例

==**IService的Lambda查询**==

```xml
<select id="queryUsers" resultType="com.itheima.mp.domain.po.User">
    SELECT *
    FROM tb_user
    <where>
        <if test="name != null">
            AND username LIKE #{name}
        </if>
        <if test="status != null">
            AND `status` = #{status}
        </if>
        <if test="minBalance != null and maxBalance != null">
        	AND balance BETWEEN #{minBalance} AND #{maxBalance}
        </if>
    </where>
</select>
```

可以替换到UserServiceImpl

```java
@Override
public List<User> queryUsers(String name, Integer status, Integer minBalance, Integer maxBalance) {
    return lambdaQuery()
            .like(name != null, User::getUsername, name)	//不为空且name模糊查询
            .eq(status != null, User::getStatus, status)	//不为空且状态相同eq
            .gt(minBalance != null, User::getBalance, minBalance)	//不为空且大于balance
            .lt(maxBalance != null, User::getBalance, maxBalance)	//不为空且小于balance
            .list();	//返回集合list
}
```

- 将复杂的WHERE条件转换成了Wrapper，并且用上了mp的IService接口
