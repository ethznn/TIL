# mysql indexing using columns

```mysql
use database_name;

CREATE INDEX idx_name_column_1_column_2 ON owner_info_details (column_1, column_2);
```

추가적으로 중복이 낮은 (카디널리티 값은 높음) column을 찾는 것이 좋다. 

인덱싱이나 쿼리로 찾을 때도 순서를 지키는 것이 좋다.