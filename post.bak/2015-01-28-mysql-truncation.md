---
title: MySQL Truncation
tags:
- mysql
- bdd
---
## Description

Here is a very interesting issue in MySQL database. 
SQL truncation occurs when you try to insert/update a field with a string which is longer than the maximum length defined in the table structure. 
For instance if you defined a column `name` as a varchar(8) and you provide `abracadabra` wich is 11, MySQL will truncate the string to 8 and will insert `abracada` instead. 
No message, no warning, nothing at all. This flaw can lead to security issue in some case.

## Example

First I create this small table:

```sql
CREATE TABLE IF NOT EXISTS `user` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `login` varchar(8) NOT NULL,
  `passwd` varchar(64) NOT NULL,
  PRIMARY KEY (`id`)
);
```

<!--more-->

Then I insert some users:

[![MySQL Truncation](/images/mysql-truncation-1.png)](/images/mysql-truncation-1.png)

My three users are ok and absolutly regular. 
But now what's happen if I try to insert another user who has a name longer that 8 (`login` max length)?

[![MySQL Truncation](/images/mysql-truncation-2.png)](/images/mysql-truncation-2.png)

See? It has been truncated to 8 without any additionnal message. So maybe I can try another one?

[![MySQL Truncation](/images/mysql-truncation-3.png)](/images/mysql-truncation-3.png)

Looks like it worked. 
Now I have a user "`admin`" and a user "`admin  ` " with some trailing spaces. 
Unfortunatly MySQL strings compare also suffers of another issue who ignores trailing spaces, that means those two strings are considered as equal in some case. POC:

[![MySQL Truncation](/images/mysql-truncation-4.png)](/images/mysql-truncation-4.png)

Et voilà! MySQL clause `group by` considers user `1` and user `4` as "equal".

Tests with operator '`=' `:
[![MySQL Truncation](/images/mysql-truncation-5.png)](/images/mysql-truncation-5.png)

Tests with operator `like `:
[![MySQL Truncation](/images/mysql-truncation-6.png)](/images/mysql-truncation-6.png)</div>

In this situation `like` seems to be safer.

## Why is it dangerous ?

First of all, it should not be allowed to register two users with the same login/email in any system. 
Then, since MySQL will return 2 results in some case, you cannot really predict wich one will be used when comparing with the user entry so it can be dangerous in some function like "Forgot password". 
A user could receive an email with credentials of another user...

## What can I do ?

According to [MySQL documentation](http://dev.mysql.com/doc/refman/5.0/en/sql-mode.html#sqlmode_strict_trans_tables "MySQL documentation"), a trick is to change `sql_mode` to be more strict:

> If a value could not be inserted as given into a transactional table, abort the statement.

[![MySQL Truncation](/images/mysql-truncation-7.png)](/images/mysql-truncation-7.png)

As a matter of fact I'm not sure this is a good solution. 
Many developers don't even test the result of their sql requests so if the server uses this parameter this can leads to many holes.

Another solution would be to use a unique index on the proper field, MySQL will trigger an error because it will (again) consider "`admin`" and "`admin  ` " as equal.

Finally, most of the orm check the good validity of the values before executing the queries and apply filters. 
This must be tested...
