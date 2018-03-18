# rails 안에서 sql query

active_record relation에는 array 연산 대신 sql query를 사용.

```ruby
@parcel.owner_info_details.exists?
```

array 연산 대산 sql query 사용.

```ruby
owner_parcels.count
```
count column unique

```ruby
owner_parcels.distinct.count(:pnu)
```

