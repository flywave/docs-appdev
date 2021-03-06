---
title: TiDB 数据库开发规范 - 数据模型设计
summary: 介绍如何设计高效的数据模型。
---

# 数据模型设计

数据库模型设计是指对于一个给定的应用环境，构造合理的数据库模式，建立数据库及其应用系统，有效存储数据，满足用户信息要求和处理要求。数据库设计在开发过程中处于一个非常重要的地位。一个高效的数据库模型是非常重要和必要的。

## 1. 完整性

数据库完整性是指数据库中数据的正确性和相容性，数据库完整性是由完整性约束来保证的，数据库完整性对于数据库应用系统非常关键，其作用主要体现在以下几个方面：

- 利用完整性控制机制来实现业务规则，易于定义，容易理解，而且可以降低应用程序的复杂性，提高应用程序的运行效率。

- 合理的数据库完整性设计，能够同时兼顾数据库的完整性和系统的效能。在应用软件的功能测试中，完善的数据库完整性有助于尽早发现应用软件的错误。

- 为了在数据库和应用程序代码之间提供另一层抽象，可以为应用程序建立专门的视图而不必非要应用程序直接访问数据表。这样做还等于在处理数据库变更时给你提供了更多的自由。

## 2. 性能

性能是衡量一个系统的关键因素，在设计阶段就在性能方面就应该多关注，尽量减少后期的烦恼。在数据库设计阶段，性能上的考虑时需要注意：不能以范式作为唯一标准或者指导，在设计过程中，需要从实际需求出发，以性能提升为根本目标来展开设计工作，一些时候为了提升性能，甚至会做反范式设计。

另外还有一些设计上的方法和技巧：

- 设置合理的字段类型和长度。字段类型在满足需求后应尽量短，比如，能用 INT 就尽量不要用 BIGINT。另外不同数据库在 VARCHAR 和 TEXT 类型在长度和性能上也是不同的，选择时要谨慎。

- 选择高效的主键和索引。由于对表记录的读取都是直接或者间接地通过主键或索引来获取，因此应该该根据具体应用特性来设计合理的主键或索引。同时索引长度的也应该关注，尽量减少索引长度。

- 适度冗余。适度的冗余可以避免关联查询，减少 join 查询。

## 3. 扩展性

在大规模系统中，除了性能，可扩展性也是设计的关键点，而数据库表扩展性主要包含表逻辑结构、功能字段的增加、分表等。在扩展性上要把握的原则如下：

- 一表一实体。如果不同实体之间有关联时，可增加一个单独的表，不会影响以前的功能。

- 分表设计，也就是水平切分。在 TiDB 目前模型设计阶段应该考虑数据的增长情况，并根据数据特性以及数据之间关系选择合适的切分策略。TiDB 当前支持的类型包括 Range 分区和 Hash 分区 等。Range 分区和 List 分区可以用于解决业务中大量删除带来的性能问题；Hash 分区则可以用于大量写入场景下的数据打散。
