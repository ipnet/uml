# Code

## Java

Demo.java
```java

package org.iproute.resource.mybatis;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

/**
 * Demo
 *
 * @author winterfell
 * @since 2021/8/31
 */
public class Demo {

    public static void main(String[] args) throws Exception {

        String resource = "mybatis-config.xml";

        InputStream inputStream = Resources.getResourceAsStream(resource);

        //创建SqlSessionFacory
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        /* *****************************分割线***************************** */
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //获取Mapper
        DemoMapper mapper = sqlSession.getMapper(DemoMapper.class);

        Map<String, Object> map = new HashMap<>();
        map.put("id", 1);
        System.out.println(mapper.queryTest(map));

        sqlSession.commit();
        sqlSession.close();

    }
}

```

DemoMapper.java
```java
package org.iproute.resource.mybatis;

import java.util.Map;

/**
 * DemoMapper
 *
 * @author winterfell
 * @since 2021/8/31
 */
public interface DemoMapper {

    /**
     * Query test map.
     *
     * @param map the map
     * @return the map
     */
    Map<String, Object> queryTest(Map<String, Object> map);
}

```

## 配置文件

mybatis-config.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 和spring整合后 environments配置将废除 -->
    <environments default="development">
        <environment id="development">
            <!-- 使用jdbc事务管理 -->
            <transactionManager type="JDBC" />
            <!-- 数据库连接池 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver" />
                <property name="url"
                          value="jdbc:mysql://172.100.1.100:3306/zhuzhenjie?characterEncoding=utf-8&amp;autoReconnect=true&amp;failOverReadOnly=false&amp;useSSL=false&amp;serverTimezone=Asia/Shanghai"/>
                <property name="username" value="root" />
                <property name="password" value="Root@123#" />
            </dataSource>
        </environment>
    </environments>

    <!-- 加载mapper.xml -->
    <mappers>
        <!-- <package name=""> -->
        <mapper resource="mapper/DemoMapper.xml"  ></mapper>
    </mappers>
</configuration>
```

mapper/DemoMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.iproute.resource.mybatis.DemoMapper">
    <select id="queryTest" parameterType="Map" resultType="Map">
        select *
        from demo
        WHERE id = #{id}
    </select>
</mapper>
```

## SQL
```sql
drop table if exists demo;

create table demo
(
    `id`   bigint       not null auto_increment,
    `name` varchar(100) not null default 'zzj',
    primary key (id)

);

insert into demo (`name`)
values ('hello'),
       ('world');
```


