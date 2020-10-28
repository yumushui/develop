
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


Douglas Crockford 于
2001 年提出，井在2006 年由IETF 通过RFC 4627 进行首次标准化。2013 年秋， Ecma 国
际通过ECMA 404 将JSON 正式标准化。


## JSON的基本概念

JSON是一种技术标准

JSON数据格式是的应用程序可以通过 RESTful API等方式进行数据通信。JSON不局限某项技术，本身非私有，且可移植。
对于产生（序列化）和读取（反序列化）JSON数据，所有的现代编程语言和平台都提供了良好的支持。
JSON非常简单，由 对象、数组、名称-值对 这三种开发人员所熟悉的结构体组成。

除了 表现层状态转化 REST，JSON还在以下环境中也有所应用：
+ Node.js （在 package.json 中存储项目元数据）；
+ MongoDB 等NoSQL数据库；
+ Kafka 等消息平台；

一个合法的 JSON 文档一般属于下面的两种情况之一：

+ 由 大括号 { 和 } 括起来的一个对象；
+ 由 中括号 [ 和 ] 括起来的一个数组。

对象和数字可以嵌套，里面包含 “key-value” 对象，key和 value 之间使用冒号 ：  隔开。

JSON 包含 3 中核心数据类型：
```
key-value 名称-值对 ： 由一个名称（数据属性）和一个值组成。
otbject 对象： 名称-值对的无序集合。
array 数组： 值的有序集合。
```

1 名称-值对
```
nameValue.json

{
    "conference": "PG China 2020",
    "speechTitle": "JSON in PostgreSQL",
    "track": "Web APIs"
}
```

2 对象
```
JsonObject.json

{
    "speaker" : {
        "firstName": "Feixiang",
        "lastName": "Zhao",
        "topics": [ "JSON", "REST", "POSTGRES" ]
    }
}
```

3 数组
```
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
```


##  为什么使用 JSON

+ 基于 JSON 的 RESTful API 的爆发式增长；
+ JSON 基本数据结构的简洁行；（逐步替代XML成为互联网上主要的数据交换格式；易于阅读，香港结构与开发人员熟悉的概念很容易对应；更适合面向对象的设计和开发；容量更小，传输和处理更快，效率更高。）
+ JavaScript 日渐流行，在配置文件领域也占有一席之地。

##  JSON 技术栈模拟

先创建一段简单的 JSON数据，用于表示会议演讲这，然后将其发布为一个模拟的 RESTful API.具体操作步骤如下：

+ （1） 用 JSON Editor Online 对 JSON 数据进行建模。
+ （2） 用 JSON Generator 生成示例数据。
+ （3） 创建并部署模拟 API，为之后的测试工作做准备。



## Postgresql 对 JSON类型 的支持

Postgresql 在 9.2 版本开始支持 json类型，后续版本对 json 的支持趋于完善。

```
select ''::json;

select ''::jsonb;


CREATE TABLE test_json_01 (id serial primary key, name json);

INSERT INTO test_json_01 (name) VALUES ('{"track": "Web APIs", "conference": "PG China 2020", "speechTitle": "JSON in PostgreSQL"}');

INSERT INTO test_json_01 (name) VALUES ('{"speaker": {"topics": ["JSON", "REST", "POSTGRES"], "lastName": "Zhao", "firstName": "Feixiang"}}');

INSERT INTO test_json_01 (name) VALUES ('{"speaker": {"topics": ["JSON", "REST", "POSTGRES"], "lastName": "Tang", "firstName": "Cheng"}}');
```

json 与 jsonb 的不同：
```
1 存储格式： json 为文本，jsonb 为二进制；
2 处理效率： json类型存储和输入相同，检索时必须重新解析；jsonb以二进制形式存储已解析好的数据，检索数据是不需要重新解析。
因为 json写入比 jsonb快，但检索 jsonb比json快。
3 输出处理： json类型输出结果与输入完全一样，有空格也不会去掉，有重复键不会删除；jsonb类型输出顺序与输入顺序不一定相同，会去掉键值中的空格，会删除回复键，值保留最后一个。

在大多数场景下，建议使用 jsonb。
```


Json 与 Jsonb 不同的对比：

```
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
```

数据查询

```
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

```

jsonb 与 json  函数：

```
SELECT * FROM json_each('{"a":"feixiang", "b":"cheng"}');

SELECT row_to_json(test_copy) FROM test_copy WHERE id=1;

SELECT * FROM json_object_keys('{"a":"foo", "b":"bar"}');


CREATE TABLE test_json_02 (id serial primary key, name jsonb);

INSERT INTO test_json_02 (name) VALUES ('{"track": "Web APIs", "conference": "PG China 2020", "speechTitle": "JSON in PostgreSQL"}');

INSERT INTO test_json_02 (name) VALUES ('{"speaker": {"topics": ["JSON", "REST", "POSTGRES"], "lastName": "Zhao", "firstName": "Feixiang"}}');

INSERT INTO test_json_02 (name) VALUES ('{"speaker": {"topics": ["JSON", "REST", "POSTGRES"], "lastName": "Tang", "firstName": "Cheng"}}');
```


jsonb 键值的 追加、删除、更新

追加数据：

```
select name -> 'speaker' || '{"job":"DBA"}';


#update test_json_02 set name = jsonb_set(name, '{speaker}', name->'speaker' || '{"job":"DBA"}' , true) where id=2;
UPDATE 1
```

删除数据

```
方式一 使用符号 '-' ：

SELECT '{"name": "Feixiang", "firstName": "Zhao", "job": "DBA"}'::jsonb - 'firstName';

SELECT '["PostgreSQL", "MySQL", "Oracle", "SQL Server"]'::jsonb - 2;


方式二 使用符号 '#-' :

SELECT '{"database": "PostgreSQL", "version": {"pg9": "9.4", "pg10": "10.14", "pg11":"11.9", "pg12":"12.4"}}'::jsonb #- '{version,pg9}'::text[];

SELECT '{"database": "PostgreSQL", "version": [ "9.4", "10.14", "11.9", "12.4" ]}' #- '{version,9.4}'::text[];
```


更新数据

```
方式一： 使用 || 操作符，连接 json 键，覆盖重复的键值

SELECT '{"database": "PostgreSQL", "version": "9.4"}'::jsonb || '{"version": "12"}'::jsonb;


方式二： 通过 jsonb_set 函数修改

jsonb_set(target jsonb, path text[], new_value jsonb [, create_missing boolean])

target 指源jsonb数据， path 指路径， new_value 指更新后的键值，
create_missing 值为布尔型，如果为 true 表示如果键不存在则添加，为 false 表示如果键不存在则不添加，示例为：

SELECT jsonb_set('{"database": "PostgreSQL", "version": "9.4"}'::jsonb, '{version}', '"12"'::jsonb, false );

SELECT jsonb_set('{"database": "PostgreSQL", "version": "9.4"}'::jsonb, '{OS}', '"CentOS 8"'::jsonb, false );

SELECT jsonb_set('{"database": "PostgreSQL", "version": "9.4"}'::jsonb, '{OS}', '"CentOS 8"'::jsonb, true );
```


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


-- jsonb 和 json 索引测试

-- create table structure
```
CREATE TABLE test_json_04_1 ( 
    id int4,
    user_id int8,
    user_name character varying(64),
    create_time timestamp(6) without time zone default clock_timestamp()
);

INSERT INTO test_json_04_1(id, user_id, user_name)
SELECT r, round(random() * 200000), r || '_francs'
FROM generate_series(1, 200000) as r;


CREATE TABLE test_json_04_2 (id serial, data json);
    
CREATE TABLE test_json_04_3 (id serial, data jsonb);
```

-- json jsonb write performance testing
```
INSERT INTO test_json_04_2(data) SELECT row_to_json(test_json_04_1) FROM test_json_04_1;

INSERT INTO test_json_04_3(data) SELECT row_to_json(test_json_04_1) FROM test_json_04_1;
```

-- json jsonb read preformance testing

按照等值查询
```
EXPLAIN ANALYZE
SELECT * FROM test_json_04_2 WHERE data->>'user_name' = '1_francs';

EXPLAIN ANALYZE
SELECT * FROM test_json_04_3 WHERE data->>'user_name' = '1_francs';

对于查询键值，创建 btree 索引：
CREATE INDEX idx_data_user_name_02 ON test_json_04_2 USING btree ((data->>'user_name'));

CREATE INDEX idx_data_user_name ON test_json_04_3 USING btree ((data->>'user_name'));
```

按照 ID 范围扫描
```
按照 ID 范围扫描，确认索引：
CREATE INDEX idx_gin_json_data_id ON test_json_04_2 USING btree (((data ->> 'id')::integer));

CREATE INDEX idx_gin_jsonb_data_id ON test_json_04_3 USING btree (((data ->> 'id')::integer));

按照 ID 范围查询
EXPLAIN ANALYZE
SELECT id, data->'id', data->'user_name' FROM test_json_04_2 WHERE (data->>'id')::int4 > 1 AND (data->>'id')::int4 < 10000;


EXPLAIN ANALYZE
SELECT id, data->'id', data->'user_name' FROM test_json_04_3 WHERE (data->>'id')::int4 > 1 AND (data->>'id')::int4 < 10000;
```


按照 key/value 进行检索
```
EXPLAIN ANALYZE
SELECT * FROM test_json_04_2 WHERE data @> '{"user_name": "2_francs"}';


EXPLAIN ANALYZE
SELECT * FROM test_json_04_3 WHERE data @> '{"user_name": "2_francs"}';

在 data 列上创建 gin 索引：
CREATE INDEX idx_gin_json_data on test_json_04_2 USING gin (data);

CREATE INDEX idx_gin_jsonb_data on test_json_04_3 USING gin (data);
```

全文检索对 json 和 jsonb 数据类型的支持

从 PostgreSQL 10 开始一个支持的新特性



上面测试的实际执行过程

```
dba_test@127.0.0.1:5432=#CREATE TABLE test_json_04_1 (
dba_test(#     id int4,
dba_test(#     user_id int8,
dba_test(#     user_name character varying(64),
dba_test(#     create_time timestamp(6) without time zone default clock_timestamp()
dba_test(# );
CREATE TABLE
Time: 8.810 ms
dba_test@127.0.0.1:5432=#select count(*) from test_json_04_1;
 count
-------
     0
(1 row)

Time: 2.314 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#INSERT INTO test_json_04_1(id, user_id, user_name)
dba_test-# SELECT r, round(random() * 200000), r || '_francs'
dba_test-# FROM generate_series(1, 200000) as r;
INSERT 0 200000
Time: 517.678 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#select count(*) from test_json_04_1;
 count
--------
 200000
(1 row)

Time: 19.335 ms

dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#CREATE TABLE test_json_04_2 (id serial, data json);
CREATE TABLE
Time: 8.978 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#CREATE TABLE test_json_04_3 (id serial, data jsonb);
CREATE TABLE
Time: 4.544 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#select -- json jsonb write performance testing;

dba_test@127.0.0.1:5432=#INSERT INTO test_json_04_2(data) SELECT row_to_json(test_json_04_1) FROM test_json_04_1;
INSERT 0 200000
Time: 1003.306 ms (00:01.003)
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#INSERT INTO test_json_04_3(data) SELECT row_to_json(test_json_04_1) FROM test_json_04_1;
INSERT 0 200000
Time: 1425.622 ms (00:01.426)

dba_test@127.0.0.1:5432=#\d+ test_json_04_2
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#\d+ test_json_04_3
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#\dt+ test_json_04_2
                           List of relations
 Schema |      Name      | Type  |     Owner     | Size  | Description
--------+----------------+-------+---------------+-------+-------------
 public | test_json_04_2 | table | feixiang.zhao | 26 MB |
(1 row)

dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#\dt+ test_json_04_3
                           List of relations
 Schema |      Name      | Type  |     Owner     | Size  | Description
--------+----------------+-------+---------------+-------+-------------
 public | test_json_04_3 | table | feixiang.zhao | 33 MB |
(1 row)

dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#select * from test_json_04_2 limit 1;
 id |                                            data
----+--------------------------------------------------------------------------------------------
  1 | {"id":1,"user_id":45875,"user_name":"1_francs","create_time":"2020-10-28T14:52:41.184963"}
(1 row)

Time: 6.504 ms
dba_test@127.0.0.1:5432=#select * from test_json_04_3 limit 1;
 id |                                               data
----+---------------------------------------------------------------------------------------------------
  1 | {"id": 1, "user_id": 45875, "user_name": "1_francs", "create_time": "2020-10-28T14:52:41.184963"}
(1 row)

Time: 1.531 ms
dba_test@127.0.0.1:5432=#


dba_test@127.0.0.1:5432=#select -- json jsonb read preformance testing
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT * FROM test_json_04_2 WHERE data->>'user_name' = '1_francs';
                                                           QUERY PLAN
--------------------------------------------------------------------------------------------------------------------------------
 Gather  (cost=1000.00..5714.00 rows=1000 width=104) (actual time=0.199..77.121 rows=1 loops=1)
   Workers Planned: 2
   Workers Launched: 2
   ->  Parallel Seq Scan on test_json_04_2  (cost=0.00..4614.00 rows=417 width=104) (actual time=42.773..67.673 rows=0 loops=3)
         Filter: ((data ->> 'user_name'::text) = '1_francs'::text)
         Rows Removed by Filter: 66666
 Planning Time: 0.048 ms
 Execution Time: 77.138 ms
(8 rows)

Time: 77.572 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT * FROM test_json_04_3 WHERE data->>'user_name' = '1_francs';
                                                          QUERY PLAN
-------------------------------------------------------------------------------------------------------------------------------
 Gather  (cost=1000.00..6505.00 rows=1000 width=135) (actual time=0.149..22.171 rows=1 loops=1)
   Workers Planned: 2
   Workers Launched: 2
   ->  Parallel Seq Scan on test_json_04_3  (cost=0.00..5405.00 rows=417 width=135) (actual time=8.502..15.058 rows=0 loops=3)
         Filter: ((data ->> 'user_name'::text) = '1_francs'::text)
         Rows Removed by Filter: 66666
 Planning Time: 0.040 ms
 Execution Time: 22.189 ms
(8 rows)

Time: 22.924 ms
dba_test@127.0.0.1:5432=#

dba_test@127.0.0.1:5432=#CREATE INDEX idx_data_user_name ON test_json_04_3 USING btree ((data->>'user_name'));
CREATE INDEX
Time: 161.770 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#CREATE INDEX idx_data_user_name_02 ON test_json_04_2 USING btree ((data->>'user_name'));
CREATE INDEX
Time: 344.110 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT * FROM test_json_04_2 WHERE data->>'user_name' = '1_francs';
                                                            QUERY PLAN
----------------------------------------------------------------------------------------------------------------------------------
 Bitmap Heap Scan on test_json_04_2  (cost=24.17..2193.57 rows=1000 width=104) (actual time=0.037..0.038 rows=1 loops=1)
   Recheck Cond: ((data ->> 'user_name'::text) = '1_francs'::text)
   Heap Blocks: exact=1
   ->  Bitmap Index Scan on idx_data_user_name_02  (cost=0.00..23.92 rows=1000 width=0) (actual time=0.034..0.034 rows=1 loops=1)
         Index Cond: ((data ->> 'user_name'::text) = '1_francs'::text)
 Planning Time: 0.783 ms
 Execution Time: 0.056 ms
(7 rows)

Time: 1.311 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT * FROM test_json_04_3 WHERE data->>'user_name' = '1_francs';
                                                          QUERY PLAN
-------------------------------------------------------------------------------------------------------------------------------
 Bitmap Heap Scan on test_json_04_3  (cost=24.17..2369.19 rows=1000 width=135) (actual time=0.036..0.037 rows=1 loops=1)
   Recheck Cond: ((data ->> 'user_name'::text) = '1_francs'::text)
   Heap Blocks: exact=1
   ->  Bitmap Index Scan on idx_data_user_name  (cost=0.00..23.92 rows=1000 width=0) (actual time=0.033..0.033 rows=1 loops=1)
         Index Cond: ((data ->> 'user_name'::text) = '1_francs'::text)
 Planning Time: 0.697 ms
 Execution Time: 0.054 ms
(7 rows)

Time: 1.169 ms



--  id range query

dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT id, data->'id', data->'user_name' FROM test_json_04_2 WHERE (data->>'id')::int4 > 1 AND (data->>'id')::int4 < 10000;
                                                            QUERY PLAN
----------------------------------------------------------------------------------------------------------------------------------
 Gather  (cost=1000.00..6966.09 rows=1000 width=68) (actual time=0.213..135.645 rows=9998 loops=1)
   Workers Planned: 2
   Workers Launched: 2
   ->  Parallel Seq Scan on test_json_04_2  (cost=0.00..5866.09 rows=417 width=68) (actual time=0.034..126.571 rows=3333 loops=3)
         Filter: ((((data ->> 'id'::text))::integer > 1) AND (((data ->> 'id'::text))::integer < 10000))
         Rows Removed by Filter: 63334
 Planning Time: 0.077 ms
 Execution Time: 136.362 ms
(8 rows)

Time: 137.471 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT id, data->'id', data->'user_name' FROM test_json_04_3 WHERE (data->>'id')::int4 > 1 AND (data->>'id')::int4 < 10000;
                                                           QUERY PLAN
---------------------------------------------------------------------------------------------------------------------------------
 Gather  (cost=1000.00..7757.09 rows=1000 width=68) (actual time=0.214..42.858 rows=9998 loops=1)
   Workers Planned: 2
   Workers Launched: 2
   ->  Parallel Seq Scan on test_json_04_3  (cost=0.00..6657.09 rows=417 width=68) (actual time=0.033..34.355 rows=3333 loops=3)
         Filter: ((((data ->> 'id'::text))::integer > 1) AND (((data ->> 'id'::text))::integer < 10000))
         Rows Removed by Filter: 63334
 Planning Time: 0.055 ms
 Execution Time: 43.585 ms
(8 rows)

Time: 44.351 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#CREATE INDEX idx_gin_json_data_id ON test_json_04_2 USING btree (((data ->> 'id')::integer));
CREATE INDEX
Time: 155.160 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#CREATE INDEX idx_gin_jsonb_data_id ON test_json_04_3 USING btree (((data ->> 'id')::integer));
CREATE INDEX
Time: 102.350 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT id, data->'id', data->'user_name' FROM test_json_04_2 WHERE (data->>'id')::int4 > 1 AND (data->>'id')::int4 < 10000;
                                                             QUERY PLAN
------------------------------------------------------------------------------------------------------------------------------------
 Bitmap Heap Scan on test_json_04_2  (cost=22.67..2212.07 rows=1000 width=68) (actual time=0.862..21.410 rows=9998 loops=1)
   Recheck Cond: ((((data ->> 'id'::text))::integer > 1) AND (((data ->> 'id'::text))::integer < 10000))
   Heap Blocks: exact=164
   ->  Bitmap Index Scan on idx_gin_json_data_id  (cost=0.00..22.42 rows=1000 width=0) (actual time=0.832..0.832 rows=9998 loops=1)
         Index Cond: ((((data ->> 'id'::text))::integer > 1) AND (((data ->> 'id'::text))::integer < 10000))
 Planning Time: 3.796 ms
 Execution Time: 22.110 ms
(7 rows)

Time: 26.414 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT id, data->'id', data->'user_name' FROM test_json_04_3 WHERE (data->>'id')::int4 > 1 AND (data->>'id')::int4 < 10000;
                                                             QUERY PLAN
-------------------------------------------------------------------------------------------------------------------------------------
 Bitmap Heap Scan on test_json_04_3  (cost=22.67..2387.69 rows=1000 width=68) (actual time=0.743..6.398 rows=9998 loops=1)
   Recheck Cond: ((((data ->> 'id'::text))::integer > 1) AND (((data ->> 'id'::text))::integer < 10000))
   Heap Blocks: exact=193
   ->  Bitmap Index Scan on idx_gin_jsonb_data_id  (cost=0.00..22.42 rows=1000 width=0) (actual time=0.714..0.714 rows=9998 loops=1)
         Index Cond: ((((data ->> 'id'::text))::integer > 1) AND (((data ->> 'id'::text))::integer < 10000))
 Planning Time: 3.106 ms
 Execution Time: 7.098 ms
(7 rows)

Time: 10.724 ms


--  验证 gin 

dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT * FROM test_json_04_2 WHERE data @> '{"user_name": "2_francs"}';
ERROR:  operator does not exist: json @> unknown
LINE 2: SELECT * FROM test_json_04_2 WHERE data @> '{"user_name": "2...
                                                ^
HINT:  No operator matches the given name and argument types. You might need to add explicit type casts.
Time: 0.361 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT * FROM test_json_04_3 WHERE data @> '{"user_name": "2_francs"}';
                                                          QUERY PLAN
-------------------------------------------------------------------------------------------------------------------------------
 Gather  (cost=1000.00..6216.67 rows=200 width=135) (actual time=0.150..27.737 rows=1 loops=1)
   Workers Planned: 2
   Workers Launched: 2
   ->  Parallel Seq Scan on test_json_04_3  (cost=0.00..5196.67 rows=83 width=135) (actual time=11.795..19.941 rows=0 loops=3)
         Filter: (data @> '{"user_name": "2_francs"}'::jsonb)
         Rows Removed by Filter: 66666
 Planning Time: 0.046 ms
 Execution Time: 27.756 ms
(8 rows)

Time: 28.636 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#CREATE INDEX idx_gin_json_data on test_json_04_2 USING gin (data);
ERROR:  data type json has no default operator class for access method "gin"
HINT:  You must specify an operator class for the index or define a default operator class for the data type.
Time: 0.445 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#CREATE INDEX idx_gin_jsonb_data on test_json_04_3 USING gin (data);
CREATE INDEX
Time: 2719.784 ms (00:02.720)
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT * FROM test_json_04_2 WHERE data @> '{"user_name": "2_francs"}';
ERROR:  operator does not exist: json @> unknown
LINE 2: SELECT * FROM test_json_04_2 WHERE data @> '{"user_name": "2...
                                                ^
HINT:  No operator matches the given name and argument types. You might need to add explicit type casts.
Time: 0.395 ms
dba_test@127.0.0.1:5432=#
dba_test@127.0.0.1:5432=#EXPLAIN ANALYZE
dba_test-# SELECT * FROM test_json_04_3 WHERE data @> '{"user_name": "2_francs"}';
                                                          QUERY PLAN
------------------------------------------------------------------------------------------------------------------------------
 Bitmap Heap Scan on test_json_04_3  (cost=37.55..696.34 rows=200 width=135) (actual time=0.079..0.079 rows=1 loops=1)
   Recheck Cond: (data @> '{"user_name": "2_francs"}'::jsonb)
   Heap Blocks: exact=1
   ->  Bitmap Index Scan on idx_gin_jsonb_data  (cost=0.00..37.50 rows=200 width=0) (actual time=0.072..0.072 rows=1 loops=1)
         Index Cond: (data @> '{"user_name": "2_francs"}'::jsonb)
 Planning Time: 0.750 ms
 Execution Time: 0.112 ms
(7 rows)

Time: 1.383 ms
dba_test@127.0.0.1:5432=#




```


