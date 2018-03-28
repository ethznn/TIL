## mysql dump



#### mysql dump 로 DB 백업 파일 만들기

ex) mysqldump -u root -p -h localhost --verbose name_of_database > /경로/.sql파일

```bash
mysqldump -u root -p -h localhost --verbose pray_development > tmp/data/backup.sql

mysqldump -u testuser -p testDB testTable > test.sql
```



------



#### mysql dump 로 DB 복구하기

ex) mysql -u root -p -h localhost --verbose name_of_database < /경로/.sql파일

```bash
mysql -u root -p -h localhost pray_development < tmp/data/backup.sql

mysql -u testuser -p testDB testTable < test.sql
```

