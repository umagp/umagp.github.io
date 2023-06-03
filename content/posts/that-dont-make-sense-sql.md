---
title: "That Don't Make Sense, SQL"
tags: ["sql"]
date: 2023-05-31T17:45:34-08:00
---
One of the things that baffles me with SQL is how some things make complete sense, but when you apply the same logic, it just doesn’t work. For example:

```sql
select *
from payments
where amount > 20;
```
 
This works and it makes sense that it works.
Then you try this:

```sql
select *
from payments
where avg(amount);
```

And you get a grouping error. Which makes no sense. Why shouldn’t you be able to make that work? Instead, you have to write it as a subquery.

```sql
select *
from payments
where amount > (
  select avg(amount) 
  from payments
);
```

But how has the former created a grouping error? You’re not grouping anything. You’re comparing each entry to the average and validating yes or no, but there’s no aggregation for the results. The answer is one of those logical leaps when compared to similar queries. 

ChatGPT says that the reason this is true is because `WHERE` works on a row level whereas `AVG()` is an aggregate function. My partner tells me this is because `AVG` doesn't have "value semantics" in SQL, but that doesn't mean much to me right now. 

That’s one of the frustrating things about SQL. While I am technically correct and following the formula, I’m wrong. When learning online, the biggest problem is that often times it’s rote and formulaic, with limited ability to ask questions or have conversations with peers about the subject matter. Learning is supplemented between my partner and ChatGPT.
