#  Data Format

+ 数据格式

#  JSON

JSON（JavaScript Object Notation） JavaScript对象表示法，已经成为 RESTful 接口设计中的事实标准，架构师和开发人员可以使用一整套现成的技术生态系统来搭建设计精巧的应用系统。

## JSON 工具：
JSON 编辑器/建模工具；
单元测试工具（如  Mocha/Chai, Minitest, JUnit ）；
JSON 校验工具；
JSON Schema 生成器；
JSON 搜索工具；
JSON 转换（模板）工具。

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






