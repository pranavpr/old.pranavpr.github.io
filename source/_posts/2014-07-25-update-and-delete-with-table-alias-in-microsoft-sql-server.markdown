---
layout: post
title: "UPDATE and DELETE with table alias in Microsoft SQL Server"
date: 2014-07-25 19:14:04 +0530
comments: true
categories: 
- MSS
- SQL
---

Last week I came across following SQL statement:

{% codeblock lang:sql %}
DELETE
FROM FIRST_TABLE A
WHERE A.KEY1 = '1'
  AND EXISTS
    (SELECT 'X'
     FROM SECOND_TABLE B
     WHERE B.KEY1 = A.KEY1
       AND B.KEY2 = A.KEY2);
{% endcodeblock %}

This SQL statement was running fine in Oracle but was failing in Microsoft SQL Server (MSS) with followiing error:

{% codeblock lang:c %}
[Microsoft][SQL Server Native Client 11.0][SQL Server]Incorrect syntax near 'A'.
{% endcodeblock %}

I became perplexed but little Googling told me that syntax for using table alias for UPDATE and DELETE in MSS is different. The above SQL statement needs to be modified as below:

{% codeblock lang:sql %}
DELETE A
FROM FIRST_TABLE A
WHERE A.KEY1 = '1'
  AND EXISTS
    (SELECT 'X'
     FROM SECOND_TABLE B
     WHERE B.KEY1 = A.KEY1
       AND B.KEY2 = A.KEY2);
{% endcodeblock %}

Observe that the table alias was specified just after DELETE keyword. Similarly, for UPDATE statement, table alias should be specified jut after UPDATE keyword.