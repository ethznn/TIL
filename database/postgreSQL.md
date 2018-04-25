# postgreSQl usage

### EC2 CLI

postgre RDS 안의 DB list 보기

```bash
$ psql -U root -l -h RDS_endpoint -p 5432
```

DB 생성

```bash
$ createdb -U root -h RDS_endpoint -p 5432 DB_name
```

DB 삭제

```bash
$ dropdb -U root -h RDS_endpoint -p 5432 DB_name
```

DB 씌우기

```bash
$ psql -U root -h RDS_endpoint -p 5432 DB_name < tmp/data/backupd.sql
```



postgre 연결 끊기

1. postgre process 보기

   ```bash
   $ ps -ef | grep postgres
   ```

2. process 강제 죽이기

   ```bash
   $ kill -9 [PID]
   ```