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

> As [@antoineguiral][antoine-twitter] pointed out on twitter, there is a MySQL specific command INSERT IGNORE that fits the situation. I have decided to add that solution to the application and added the results below. Thanks Antoine! ;)

## Solutions 

First of all, let's define the problem technically : I have data I want to insert into a table. That table has a unique constraint on one of the fields. If the string is already present, do nothing, else insert the string into the table. 

1. The most obvious solution would be to check if the string is already present by performing a <span class="syntax">select count(\*)</span> query. If that returns 0, insert the data. 
2. The second solution is to delegate the unique check to the database. Just insert the data and check if an exception is thrown for a "**unicity violation**".
3. The last possibility is to use a MySQL specificity : [INSERT IGNORE][mysql-insert-ignore]. If the key is not found in the table, it will perform an insert otherwise, well, it simply ignores. As Slick isn't able to do this using functions or methods, it will be done in raw SQL. 

These three solutions are present in my testing app, [published on Github][repo-github]. A few more details :

- I have chosen a duplicate/insertion ratio close to my real use-case : 500 duplicates for 1000 strings.
- I use [Slick][slick-typesafe] as in my real world application
- Compiled on Play-2.2.0-M2 (latest version)
- I am a complete beginner on Slick, there will probably be all kinds of optimizations that can be done, PR's are welcome! 

## Results

Actually, there is no surprise. It is way faster to insert all things and just catch the unique violation exception. The only surprise is in how much faster. I have tried this a dozen of times and the "with-read" solution sometimes went above 10 seconds while the other solutions never took longer than 2 seconds. And they are generally about 2 or 3 times faster. But since all you want is numbers here they are :

- **With** : 1848 ms
- **Without** : 689 ms
- **Raw** : 522 ms

As you can see, using the MySQL INSERT IGNORE command, it is even slightly faster. But you have to go through the hassle of maintaning a raw SQL command. If you are 100ms short and want screaming performance, you might want to go the extra mile for raw SQL. Otherwise, the INSERT surrounded with a Try/Catch seems fast enough for most use-cases.

## Why ? 

When first checking if the string is present, you perform twice as much SQL queries as for the second solution. **And IO is expensive**. Period.

[antoine-twitter]: https://twitter.com/antoineguiral
[mysql-insert-ignore]: http://dev.mysql.com/doc/refman/5.5/en/insert.html
[repo-github]: https://github.com/iammichiel/mysql-insert-test
[slick-typesafe]: http://slick.typesafe.com/
