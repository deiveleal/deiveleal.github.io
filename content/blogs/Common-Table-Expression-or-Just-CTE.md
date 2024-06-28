---
title: "Common Table Expression or Just CTE"
date: 2024-06-28T16:00:00+05:30
draft: false
github_link: ""
author: "Deive Leal"
tags:
  - CTE
  - Data Engineering
  - SQL
  - Database
  - SQL Tool
image: /images/projects/cte/cte.jpg
description: ""
toc: 
---

When I started my studies in sql to handle data from databases many things seemed confused and messed up. The use of nested queries or subqueries was kind of confusing. The longer the query is more difficult, it is to understand what it does. Consequently, its reading, debugging and maintenance are equally difficult. For instance, imagine a code with seven or more different subqueries. The limit is your imagination. But, the more subqueries you have, more business rules can be applied and, consequently, the challenge to maintain them increases. To help us with this great problem, a good option is to use common table expressions or aka cte.

Common table expressions appeared in sql in 1999, but these implements came from 2007. There are two kinds of cte: recursive and non-recursive. The non-recursive approach is closer to our day-by-day uses, but we will try to explain both and what problem they solve. Then, you can decide which the best option is. First, we will approach the recursive way and after we will explain the non-recursive form.

The recursive common table expression is a kind of query and permits it to refer to itself until obtaining a result. The syntax between recursive and non-recursive cte is similar. Both start with the reserved word WITH, but differently from non recursive, the next word used is RECURSIVE, which is only used in this approach. After that, you can provide a name to your cte, write the reserved word AS and, finally, between parenthesis, type at least three main parts: anchor member, recursive member and a connector between both. Finally, you can use and apply the result in another query. The common usage for cte in a recursive form is in hierarchical data such as “displaying employees in an organizational chart, or data in a bill of materials scenario in which a parent product has one or more components and those components may, in turn, have subcomponents or may be components of other parents” (Microsoft).

Example:

```SQL
WITH RECURSIVE cte_name AS (
  cte_anchor_member
  UNION
  cte_recursive_member
)
SELECT * FROM cte_name
```

The non-recursive mode is more common among data engineers who need to work with long and complex queries. I learned it when I needed to work a long query and a senior data engineer shared this approach, thanks Valmur! But, what is this kind of CTE? In the MYSql website we found that CTE (non-recursive) “is a named temporary result set that exists within the scope of a single statement and that can be referred to later within that statement, possibly multiple times”. The syntaxe in non-recursive form is simple when you know the recursive way. It’s necessary just not to use the reserved word RECURSIVE. So, in general it is: word reserved WITH, the cte-name, the word reserved AS, the query inside parenthesis and a query that uses a cte developed. As MYSql says, it’s common that a cte is called in different parts from the main query or many times for main and/or other cte queries.

Example:

```SQL
WITH cte_name_1 AS (
  select col1, col2
  from table1
),
cte_name_2 AS (
  select col3, col4
  from table2
),
cte_name_3 AS (
  select col2, col3
  from cte_name_1
  INNER JOIN cte_name_2
  ON cte_name_1.col1 = cte_name_2.col4
)
SELECT col2, col3 FROM cte_name_3
```

In conclusion, if you work with complex queries and it’s been difficult to understand the context after some time or you need to improve the readability, try using cte. Probably, you will adopt this kind of development approach to queries on a regular basis.

References:

https://learnsql.com.br/blog/o-que-e-um-cte-recursivo-em-sql/

https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms186243(v=sql.105)?redirectedfrom=MSDN

https://mariadb.com/kb/en/recursive-common-table-expressions-overview/

https://dev.mysql.com/doc/refman/8.0/en/with.html