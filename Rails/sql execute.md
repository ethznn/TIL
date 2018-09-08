# Sql execute

Using PostGis



예) 해당 @parcel 로 주변 일정 거리에 속하는 폴리곤을 가진 필지 데이터를 가져오기

ex) Getting datas of parcels that has polygons with same distanse 

```ruby
# ActiveRecord => postgre with PostGis

ActiveRecord::Base.connection.execute(
  "SELECT b FROM parcels a, parcels b 
	WHERE a.pnu='#{@parcel.pnu}' and ST_DWithin( a.polygon, b.polygon, 0.001) 
	ORDER BY 'POINT(#{@parcel.longitude} #{@parcel.latitude})' <-> b.polygon;"
)
```

