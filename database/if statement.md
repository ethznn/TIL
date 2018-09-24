# if statement

```sql
IF <조건> THEN
    <참의 실행 코드>
ELSE
    <거짓의 실행 코드>
END IF;
```

```sql
DO $$
DECLARE
    a integer := 20;
    b integer := 40;
    c integer := 20;
BEGIN 
    IF a > b THEN
        RAISE NOTICE 'a가 b보다 더 큽니다다.';
    END IF;
 
    IF a < b THEN
        RAISE NOTICE 'a가 b보다 더 작습니다.';
    END IF;
 
    IF a = b THEN
        RAISE NOTICE 'a와 b가 동일합니다.';
    END IF;
    
    IF a >= c THEN
        RAISE NOTICE 'a가 c보다 크거나 같습니다.';
    END IF;
    
    IF a != b THEN
        RAISE NOTICE 'a가 b가 같지 않습니다.';
    END IF;
    
    IF a != b AND a = c THEN
        RAISE NOTICE 'a와 b가 같지 않고 a와 c가 같습니다.';
    END IF;
    
    IF a = b OR a = c THEN
        RAISE NOTICE 'a와 b가 같거나 a와 c가 같습니다.';
    END IF;
    
    IF NOT (a = b OR a = c) THEN
        RAISE NOTICE 'a와 b가 같지지 않고 a와 c도 같지 않습니다.';
    END IF;
END $$;
```

