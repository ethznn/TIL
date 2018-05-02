# geokit usage

예시코드 =>

```ruby
Parcel
	.within(1, origin: [parcel.latitude, parcel.longitude])
	.joins(:info).where(parcel_infos: { jimok: "대" }).each do |index|
    data << index.info
  end
```

