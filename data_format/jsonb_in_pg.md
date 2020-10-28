
#  JSON

JSON（JavaScript Object Notation） JavaScript对象表示法，已经成为 RESTful 接口设计中的事实标准，架构师和开发人员可以使用一整套现成的技术生态系统来搭建设计精巧的应用系统。

## JSON 工具：
```
JSON 编辑器/建模工具；
单元测试工具（如  Mocha/Chai, Minitest, JUnit ）；
JSON 校验工具；
JSON Schema 生成器；
JSON 搜索工具；
JSON 转换（模板）工具。
```

## JSON的基本概念

JSON是一种技术标准

JSON数据格式是的应用程序可以通过 RESTful API等方式进行数据通信。JSON不局限某项技术，本身非私有，且可移植。
对于产生（序列化）和读取（反序列化）JSON数据，所有的现代编程语言和平台都提供了良好的支持。
JSON非常简单，由 对象、数组、名称-值对 这三种开发人员所熟悉的结构体组成。

除了 表现层状态转化 REST，JSON还在以下环境中也有所应用：
Node.js （在 package.json 中存储项目元数据）；
MongoDB 等NoSQL数据库；
Kafka 等消息平台；

一个合法的 JSON 文档一般属于下面的两种情况之一：
由 大括号 { 和 } 括起来的一个对象；
由 中括号 [ 和 ] 括起来的一个数组。

对象和数字可以嵌套，里面包含 “key-value” 对象，key和 value 之间使用冒号 ：  隔开。

JSON 包含 3 中核心数据类型：
名称-值对 ： 由一个名称（数据属性）和一个值组成。
对象： 名称-值对的无序集合。
数组： 值的有序集合。

1 名称-值对
nameValue.json

{
    "conference": "PG China 2020",
    "speechTitle": "JSON in PostgreSQL",
    "track": "Web APIs"
}

2 对象
JsonObject.json

{
    "speaker" : {
        "firstName": "Feixiang",
        "lastName": "Zhao",
        "topics": [ "JSON", "REST", "POSTGRES" ]
    }
}

3 数组

JsonArray.json

{
    "speaker" : {
        "firstName": "Feixiang",
        "lastName": "Zhao",
        "topics": [ "JSON", "REST", "POSTGRES" ]
        "address" : {
            "city": "Shanghai",
            "country": "China"
        }
    },
    "database" : [
        {"name": PostgreSQL,
        "version": [ "9.4", "10", "11", "12" ]},
        {"name": MySQL,
        "version": [ "5.5", "5.6", "5.7", "8" ]}
    ]
}


##  为什么使用 JSON

基于 JSON 的 RESTful API 的爆发式增长；
JSON 基本数据结构的简洁行；（逐步替代XML成为互联网上主要的数据交换格式；易于阅读，香港结构与开发人员熟悉的概念很容易对应；更适合面向对象的设计和开发；容量更小，传输和处理更快，效率更高。）
JavaScript 日渐流行，在配置文件领域也占有一席之地。

##  JSON 技术栈模拟

先创建一段简单的 JSON数据，用于表示会议演讲这，然后将其发布为一个模拟的 RESTful API.具体操作步骤如下：
（1） 用 JSON Editor Online 对 JSON 数据进行建模。
（2） 用 JSON Generator 生成示例数据。
（3） 创建并部署模拟 API，为之后的测试工作做准备。



## Postgresql 对 JSON类型 的支持

Postgresql 在 9.2 版本开始支持 json类型，后续版本对 json 的支持趋于完善。

select ''::json;

select ''::jsonb;


CREATE TABLE test_json_01 (id serial primary key, name json);

INSERT INTO test_json_01 (name) VALUES ('{"track": "Web APIs", "conference": "PG China 2020", "speechTitle": "JSON in PostgreSQL"}');

INSERT INTO test_json_01 (name) VALUES ('{"speaker": {"topics": ["JSON", "REST", "POSTGRES"], "lastName": "Zhao", "firstName": "Feixiang"}}');

INSERT INTO test_json_01 (name) VALUES ('{"speaker": {"topics": ["JSON", "REST", "POSTGRES"], "lastName": "Tang", "firstName": "Cheng"}}');

json 与 jsonb 的不同：
1 存储格式： json 为文本，jsonb 为二进制；
2 处理效率： json类型存储和输入相同，检索时必须重新解析；jsonb以二进制形式存储已解析好的数据，检索数据是不需要重新解析。
因为 json写入比 jsonb快，但检索 jsonb比json快。
3 输出处理： json类型输出结果与输入完全一样，有空格也不会去掉，有重复键不会删除；jsonb类型输出顺序与输入顺序不一定相同，会去掉键值中的空格，会删除回复键，值保留最后一个。

在大多数场景下，建议使用 jsonb。

dba_test@127.0.0.1:5432=#select '{
dba_test'#     "speaker" : {
dba_test'#         "firstName": "Feixiang",
dba_test'#         "lastName": "Zhao",
dba_test'#         "topics": [ "JSON", "REST", "POSTGRES" ]
dba_test'#     }
dba_test'# }'::json;
                       json
--------------------------------------------------
 {                                               +
     "speaker" : {                               +
         "firstName": "Feixiang",                +
         "lastName": "Zhao",                     +
         "topics": [ "JSON", "REST", "POSTGRES" ]+
     }                                           +
 }
(1 row)

dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#select '{
    "speaker" : {
        "firstName": "Feixiang",
        "lastName": "Zhao",
        "topics": [ "JSON", "REST", "POSTGRES" ]
    }
}'::jsonb;
                                               jsonb
----------------------------------------------------------------------------------------------------
 {"speaker": {"topics": ["JSON", "REST", "POSTGRES"], "lastName": "Zhao", "firstName": "Feixiang"}}
(1 row)



数据查询
dba_test@127.0.0.1:5432=#select name -> 'speaker' ->> 'firstName' as firstname from test_json_01 where id=2;
 firstname
-----------
 Feixiang
(1 row)

dba_test@127.0.0.1:5432=#select name -> 'speaker' ->> 'firstName' as firstname from test_json_01 where name->'speaker'->>'lastName' = 'Tang';
 firstname
-----------
 Cheng
(1 row)

jsonb 与 json  函数：

SELECT * FROM json_each('{"a":"feixiang", "b":"cheng"}');

SELECT row_to_json(test_copy) FROM test_copy WHERE id=1;

SELECT * FROM json_object_keys('{"a":"foo", "b":"bar"}');


CREATE TABLE test_json_02 (id serial primary key, name jsonb);

INSERT INTO test_json_02 (name) VALUES ('{"track": "Web APIs", "conference": "PG China 2020", "speechTitle": "JSON in PostgreSQL"}');

INSERT INTO test_json_02 (name) VALUES ('{"speaker": {"topics": ["JSON", "REST", "POSTGRES"], "lastName": "Zhao", "firstName": "Feixiang"}}');

INSERT INTO test_json_02 (name) VALUES ('{"speaker": {"topics": ["JSON", "REST", "POSTGRES"], "lastName": "Tang", "firstName": "Cheng"}}');


jsonb 键值的 追加、删除、更新

追加：

select name -> 'speaker' || '{"job":"DBA"}';


#update test_json_02 set name = jsonb_set(name, '{speaker}', name->'speaker' || '{"job":"DBA"}' , true) where id=2;
UPDATE 1

删除 方式一 使用符号 '-' ：

SELECT '{"name": "Feixiang", "firstName": "Zhao", "job": "DBA"}'::jsonb - 'firstName';

SELECT '["PostgreSQL", "MySQL", "Oracle", "SQL Server"]'::jsonb - 2;


删除 方式二 使用符号 '#-' :

SELECT '{"database": "PostgreSQL", "version": {"pg9": "9.4", "pg10": "10.14", "pg11":"11.9", "pg12":"12.4"}}'::jsonb #- '{version,pg9}'::text[];

SELECT '{"database": "PostgreSQL", "version": [ "9.4", "10.14", "11.9", "12.4" ]}' #- '{version,9.4}'::text[];


键值更新也有两种方式：

方式一： 使用 || 操作符，连接 json 键，覆盖重复的键值

SELECT '{"database": "PostgreSQL", "version": "9.4"}'::jsonb || '{"version": "12"}'::jsonb;


方式二： 通过 jsonb_set 函数修改

jsonb_set(target jsonb, path text[], new_value jsonb [, create_missing boolean])

target 指源jsonb数据， path 指路径， new_value 指更新后的键值，
create_missing 值为布尔型，如果为 true 表示如果键不存在则添加，为 false 表示如果键不存在则不添加，示例为：

SELECT jsonb_set('{"database": "PostgreSQL", "version": "9.4"}'::jsonb, '{version}', '"12"'::jsonb, false );

SELECT jsonb_set('{"database": "PostgreSQL", "version": "9.4"}'::jsonb, '{OS}', '"CentOS 8"'::jsonb, false );

SELECT jsonb_set('{"database": "PostgreSQL", "version": "9.4"}'::jsonb, '{OS}', '"CentOS 8"'::jsonb, true );


索引 GIN

jsonb类型支持 GIN 索引
```
CREATE TABLE test_json_03 (id serial primary key, data jsonb);

INSERT INTO test_json_03(data) values('{"id": 1, "user_id": "0dd4bed2-775f-4cad-b711-1c61b1c580d5", "user_name": "Feixiang Zhao", "create_time": "2020-09-28T14:11:11+0000"}');
INSERT INTO test_json_03(data) values('{"id": 1, "user_id": "0dd4bed2-775f-4cad-b711-1c61b1c580f3", "user_name": "Zhenping Zhao", "create_time": "2020-09-28=7T14:11:11+0000"}');

{
    "id": 1,
    "user_id": "0dd4bed2-775f-4cad-b711-1c61b1c580d5",
    "user_name": "Feixiang Zhao",
    "create_time": "2020-09-28T14:11:11+0000"
}
```

CREATE INDEX idx_gin_data ON test_json_03 USING gin(data);

CREATE INDEX idx_gin_data_user_name ON test_json_03 USING btree ((data ->> 'user_name'));

SELECT * FROM test_json_03 WHERE data @> '{"user_name": "Feixiang Zhao"}';

SELECT * FROM test_json_03 WHERE data->>'user_name' = 'Feixiang Zhao';


更多用法和函数，参见官方文档：
https://www.postgresql.org/docs/12/datatype-json.html


