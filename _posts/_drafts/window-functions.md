---
title:  "SQL and Window Functions - a quick introduction"
categories: [SQL]
tags: [sql, window, function]
classes: wide
---

Anmerkungen
Vergleich zu group by
z.B. max, avg
erst ausgewertet nach dem where (only selected rows are visible)


-- group by


 
-- window function



Syntax
window_function (expression) OVER (
   [ PARTITION BY expr_list ]
   [ ORDER BY order_list ][ frame_clause ] ) 

![Window Function Structure](assets/window-function-structure.png)
 
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