---

---
# Performing set operations UNION, INTERSECT, MINUS with Looker BI tool

## Stating the problem

I would like to list and count all the users who have used product A but never used product B (in a specific period/area/country).

Being more specific, imagine this data structure: journey, passenger, service group.

![](https://habrastorage.org/webt/g7/y7/_m/g7y7_mr7mhuagangfiztlxfepsm.png)

You cannot achieve the desired result by simply slicing this table. One needs a more advanced technique.

## Plain SQL solution

Of course this task could be easily solved by writing some SQL code.

Several approaches are valid:

* You may leverage MINUS operation on top of two sets of users: those who have used product A MINUS those who have used product B.
* You can write a subquery with IN or EXISTS keywords.
* Joining a fact table on itself with proper filters (in this case anti-join) also works.

## Can I do this with BI tool?

In case I don’t have enough SQL skills? Or don’t even have the direct database access.

Also I might want to still retain pre-defined dimensions, measures and filters for which we all love BI tools.

Looker is a very powerful instrument. Let us try to accomplish this task by performing MERGE operation.

First describe primary query to get the base segment: