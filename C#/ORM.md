# 什么是 ORM ?

ORM 是 Object Relational Mapping 的缩写，译为“对象关系映射”，是一种程序设计技术，用于实现面向对象编程语言里不同类型系统的数据之间的转换。它解决了对象和关系型数据库之间的数据交互问题，ORM的作用是在关系型数据库和业务实体对象之间作一个映射，这样我们在具体的操作业务对象的时候，就不需要再去和复杂的SQL语句打交道，只需简单的操作对象的属性和方法。

## **EF/EF Core**

> Entity Framework (EF) Core 是轻量化、可扩展、开源和跨平台版的常用 Entity Framework 数据访问技术，EF Core 是适用于 .NET 的现代对象数据库映射器。它支持 LINQ 查询、更改跟踪、更新和架构迁移。EF Core 通过提供程序插件 API 与 SQL Server、Azure SQL 数据库、SQLite、Azure Cosmos DB、MySQL、PostgreSQL 和其他数据库一起使用。(微软官方出品)。

- 官方文档教程：[https://docs.microsoft.com/zh-cn/ef/](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/ef/)
- GitHub地址：[https://github.com/dotnet/efcore](https://link.zhihu.com/?target=https%3A//github.com/dotnet/efcore)

## **Dapper**

> Dapper是一个简单的.NET对象映射器，在速度方面具有"King of Micro ORM"的头衔，几乎与使用原始的[http://ADO.NET](https://link.zhihu.com/?target=http%3A//ADO.NET)数据读取器一样快。ORM是一个对象关系映射器，它负责数据库和编程语言之间的映射。Dapper通过扩展IDbConnection提供一些有用的扩展方法去查询您的数据库。

- GitHub地址：[https://github.com/DapperLib/Dapper](https://link.zhihu.com/?target=https%3A//github.com/DapperLib/Dapper)

## **SqlSugar**

> SqlSugar 是一款 老牌 .NET 开源多库架构ORM框架（EF Core单库架构），由果糖大数据科技团队 维护和更新 ，开箱即用最易上手的.NET ORM框架 。

- 官网地址：[http://www.donet5.com](https://link.zhihu.com/?target=http%3A//www.donet5.com)
- GitHub地址：[https://github.com/donet5/SqlSugar](https://link.zhihu.com/?target=https%3A//github.com/donet5/SqlSugar)

## **FreeSql**

> FreeSql 是一款功能强大的对象关系映射（O/RM）组件，支持 .NET Core 2.1+、.NET Framework 4.0+ 以及 Xamarin。

- GitHub地址：[https://github.com/dotnetcore/FreeSql](https://link.zhihu.com/?target=https%3A//github.com/dotnetcore/FreeSql)

## **Chloe.ORM**

> Chloe.ORM 是一款国产十分稳定可靠的 ORM 框架。除了常规增删查改外还支持连接查询、分组查询、聚合查询、子查询，大部分操作可通过 lambda 完成。还支持分库分表分页、聚合、分组聚合，并支持多个字段组合分片以及多字段路由。

- 文档地址：https ://[http://github.com/shuxinqin/Chloe/wiki](https://link.zhihu.com/?target=http%3A//github.com/shuxinqin/Chloe/wiki)
- GitHub地址：[https://github.com/shuxinqin/Chloe](https://link.zhihu.com/?target=https%3A//github.com/shuxinqin/Chloe)

## **nhibernate-core**

> NHibernate是.NET框架的成熟、开源的对象关系映射工具。它在积极开发中，功能齐全，并已成功应用于数千个项目中。

- NHibernate社区网站: [https://nhibernate.info](https://link.zhihu.com/?target=https%3A//nhibernate.info)
- GitHub地址：[https://github.com/nhibernate/nhibernate-core](https://link.zhihu.com/?target=https%3A//github.com/nhibernate/nhibernate-core)

## **SmartSql**

> SmartSql = C# 中的 MyBatis + .NET Core+ 缓存（内存 | Redis）+ R/W 拆分 + PropertyChangedTrack +动态存储库 + InvokeSync + 诊断。SmartSql 借鉴了 MyBatis 的思想，使用 XML 来管理 SQL ，并且提供了若干个筛选器标签来消除代码层面的各种 if/else 的判断分支。SmartSql将管理你的 SQL ，并且通过筛选标签来维护本来你在代码层面的各种条件判断，使你的代码更加优美。

- 文档地址： [https://smartsql.net/guide/](https://link.zhihu.com/?target=https%3A//smartsql.net/guide/)
- GitHub地址：[https://github.com/dotnetcore/SmartSql](https://link.zhihu.com/?target=https%3A//github.com/dotnetcore/SmartSql)

## **PetaPoco**

> PetaPoco 是一个用于 .NET（4、4.5+、net standard 2.0+）和 Mono 的微型、快速、易于使用的 micro-ORM。由于 PetaPoco 所代表的简单性和易用性，它受到许多人的喜爱。PetaPoco 是首选的微 ORM，也是任何体面的开发人员工具包中必不可少的实用程序。

- 文档地址：[https://discoverdot.net/projects/peta-poco](https://link.zhihu.com/?target=https%3A//discoverdot.net/projects/peta-poco)
- GitHub地址：[https://github.com/CollaboratingPlatypus/PetaPoco](https://link.zhihu.com/?target=https%3A//github.com/CollaboratingPlatypus/PetaPoco)

## **linq2db**

> LINQ to DB 是最快的LINQ数据库访问库，在POCO对象和数据库之间提供了一个简单、轻量、快速且类型安全的层。在架构上，它比 Dapper、Massive 或 PetaPoco 等微 ORM 高出一步，因为您使用 LINQ 表达式，而不是魔术字符串，同时在代码和数据库之间维护一个薄抽象层。您的查询由 C# 编译器检查并允许轻松重构。但是，它不像 LINQ to SQL 或实体框架那么重。没有更改跟踪，因此您必须自己进行管理，但从积极的方面来说，您可以获得更多控制权并更快地访问您的数据。

- 文档地址：[https://linq2db.github.io/](https://link.zhihu.com/?target=https%3A//linq2db.github.io/)
- GitHub地址：[https://github.com/linq2db/linq2db](https://link.zhihu.com/?target=https%3A//github.com/linq2db/linq2db)

## **RepoDb**

> RepoDB是一个开源的.NET ORM库，它弥合了微ORM和完整ORM之间的差距。它帮助您简化在开发过程中何时使用基本操作和高级操作的切换。

- GitHub地址：[https://github.com/mikependon/RepoDB](https://link.zhihu.com/?target=https%3A//github.com/mikependon/RepoDB)

## **ServiceStack.OrmLite**

> OrmLite是一个快速、简单、类型化的.NET ORM，OrmLite 的目标是提供一个方便、DRY、无配置、与 RDBMS 无关的类型包装器，该包装器与 SQL 保持高度亲和性，公开直观的 API，生成可预测的 SQL 并干净地映射到断开连接和数据传输对象 (DTO) 友好、普通的旧C# 对象 (POCO)。这种方法更容易推理您的数据访问，从而清楚地知道什么 SQL 在什么时间执行，同时减轻意外行为、隐式 N+1 查询和重对象关系映射器 (ORM) 中普遍存在的泄漏数据访问。

- 文档地址：[https://docs.servicestack.net/ormlite/](https://link.zhihu.com/?target=https%3A//docs.servicestack.net/ormlite/)
- GitHub地址：[https://github.com/ServiceStack/ServiceStack.OrmLite](https://link.zhihu.com/?target=https%3A//github.com/ServiceStack/ServiceStack.OrmLite)

## **SQLite-net**

> 简单、强大、跨平台的 SQLite 客户端和 .NET 的 ORM。

- GitHub地址：[https://github.com/praeclarum/sqlite-net](https://link.zhihu.com/?target=https%3A//github.com/praeclarum/sqlite-net)

## **Insight.Database**

> Insight.Database是一个用于 .NET 的快速、轻量级的 micro-orm。

- GitHub地址：[https://github.com/jonwagner/Insight.Database](https://link.zhihu.com/?target=https%3A//github.com/jonwagner/Insight.Database)

## **cyqdata**

> cyq.data是一个高性能且功能最强大的orm（支持.NET Core），支持Txt、Xml、Access、Sqlite、Mssql、Mysql、Oracle、Sybase、Postgres、DB2、Redis、MemCache。

- GitHub地址：[https://github.com/cyq1162/cyqdata](https://link.zhihu.com/?target=https%3A//github.com/cyq1162/cyqdata)

## **querybuilder**

> SQL 查询构建器，用 c# 编写，帮助您轻松构建复杂的查询，支持 SqlServer、MySql、PostgreSql、Oracle、Sqlite 和 Firebird。

- 官网地址：[https://sqlkata.com/](https://link.zhihu.com/?target=https%3A//sqlkata.com/)
- GitHub地址：[https://github.com/sqlkata/querybuilder](https://link.zhihu.com/?target=https%3A//github.com/sqlkata/querybuilder)

## **TinyORM**

> TinyORM是一个简单、快速且安全的微型.NET ORM。

- Wiki地址：[https://github.com/sdrapkin/SecurityDriven.TinyORM/wiki](https://link.zhihu.com/?target=https%3A//github.com/sdrapkin/SecurityDriven.TinyORM/wiki)
- GitHub地址：[https://github.com/sdrapkin/Sec](https://link.zhihu.com/?target=https%3A//github.com/sdrapkin/SecurityDriven.TinyORM)