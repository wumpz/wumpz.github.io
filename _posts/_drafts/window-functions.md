---
title:  "SQL and Window Functions - a quick introduction"
categories: [SQL]
tags: [sql, window, function]
classes: wide
---

Since I use SQLs window function nearly on a daily basis. So there are a lot of introductions to this useful functionalty, that never got to the point IMHO. So I will try it better :). 

# Syntax

```sql
window_function (expression) OVER (
   [ PARTITION BY expr_list ]
   [ ORDER BY order_list ][ frame_clause ] ) 
```

> Note: a window function may only used in the **select** part of your statement and not in the other parts like **where**. It will evaluated **after** the your query has collected all rows, meaning, that every filter process, **where** expression and so on is finished.

```sql
select sum(my_value) over () from my_table
```

# Window Functions

As **window_function** (look at syntax) you can use some expressions, you already know from aggregate functions, like **sum, avg, min, max, ...**. And the **partition by** seems to somehow grouping your data. I think that is the reason, why the first understanding of window functions often is like: it is some kind of **group by** expression. This way of thinking prevents the real understanding of what these expressions really are. 

Lets come from the other side. Why are these functions called windows functions? 

![Window Function Structure](assets/window-function-structure.png)

You see in this picture the data rows of your **select** or **table**. Using the **partition by** clause you are able to split your data rows into partitions, sets, groups, you name it. But thats not all. The last but most important thing is the window, the range or the look at this partition, that selects a part of this partition dependent of the current row you are in, that is used to feed the **window_function**. This last part (IMHO) is the thing that gave those window functions its name. This range could be but doesn't need to be the whole partition. You are able to configure this window, there are meaningfull default definitions of it.



-- group by


 
-- window function




 
Bsp with partition only


max
min
sum (but with order by)
avg
count
Bsp with order by:
row_number, (Rechnungspositionen)
rank, dense_rank (equal values -> equal rank) (Rang von Dingen, siehe Siegertreppchen)
sum (again with order running sum) 
Laufende Summen in drei Datenbanken
Bsp with order by (access to other rows)
lead, lag (expr, [ offset, [ defaultvalue ] ]) 
vorher / nachher in drei Datenbanken
first_value, last_value, 
range definitions or frame definitions
rows
rows between unbounded preceding and unbounded following (alle)
rows between unbounded preceding and current row (std für order by)
rows between 2 preceding and 2 following
range
same as rows but only with values