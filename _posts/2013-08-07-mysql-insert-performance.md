---
layout: post
category: article 
title: Insert implementation performance on Mysql
comments: true
---

# Insert implementation performance on Mysql

{% excerpt %}

Currently, I am working on a heavy read/write database application in Play (teasing: will be online in a couple of days!) and I was faced with a classic performance dillema. I have a lot of data to insert in my Mysql database but a lot of that data could be a duplicate of data already in the table and in that case, I don't want it. I was wondering what would be the best implementation for this kind of situation and decided to create a little benchmark.

{% endexcerpt %}

## Solutions 

First of all, let's define the problem technically : I have data I want to insert into a table. That table has a unique constraint on one of the fields. If the string is already present, do nothing, else insert the string into the table. 

1. The most obvious solution would be to check if the string is already present by performing a <span class="syntax">select count(*)</span> query. If it is not, then insert it. 
2. The other solution is to delegate the unique check to the database. Just insert the data and check if an exception is thrown. 

Both of those solutions are present in my testing app, [published on Github][repo-github]. A few more details :

- I have chosen a duplicate/insertion ratio close to my real use-case : 500 duplicates for 1000 strings.
- I use [Slick][slick-typesafe] as in my real world application
- Compiled on Play-2.2.0-M2 (latest version)

## Results

Actually, there is no surprise. It is way faster to insert all things and just catch the unique violation exception. The only surprise is in how much faster. I have tried this a dozen of times and the "with-read" solution sometimes went above 10 seconds while the other solution never took longer than 2 seconds. And generally the second solution is about 2 or 3 times faster. But since all you want is numbers here they are :

- **With** : 1848 ms
- **Without** : 689 ms

## Why ? 

When first checking if the string is present, you perform twice as much SQL queries as for the second solution. **And IO is expensive**. Period.

[repo-github]: https://github.com/iammichiel/mysql-insert-test
[slick-typesafe]: http://slick.typesafe.com/
