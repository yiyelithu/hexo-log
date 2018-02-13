---
title: myBatis笔记
date: 2017-11-14 21:59:44
categories: 服务器
tags: [mybatis,Spring]
---
### mybatis foreach标签
> foreach 标签中 item属性名如果和其他参数中同名(如以下代码:item="id" 和 if test="id != null" 同名),即使没有传入id参数,SQL也会执行 AND id = #{id}
#### mapper文件:
```xml
    <select id="findPageList" parameterType="map" resultType="user">
        SELECT *
        FROM user
        <where>
            <if test="IN_id != null">
                id IN
                <foreach collection="IN_id" index="index" item="id" open="(" separator="," close=")">
                    #{id}
                </foreach>
            </if>
            <if test="id != null">
              AND id = #{id}
            </if>
        </where>
    </select>
```
#### dao层
```java
    List<User> findPageList(Map<String, Object> map);
```
#### 测试方法
```xml
    @Test
    public void test2() {
        Map<String, Object> map = new HashMap<String, Object>();
        List<Integer> idList = new ArrayList<Integer>();
        idList.add(1);
        idList.add(4);
        idList.add(5);
        map.put("IN_id", idList);
        userMapper.findPageList(map);
    }
```
#### 结果
````java
==>  Preparing: SELECT * FROM user WHERE id IN ( ? , ? , ? ) AND id = ? 
==> Parameters: 1(Integer), 4(Integer), 5(Integer), 5(Integer)
<==      Total: 1
````