# chewy / elatsicsearch

```bash
$ bin/raike chewy:reset
$ bin/raike chewy:reset[development_map] only writeen index
```

예시 definition

```ruby
def function(address, input_2)
  center_parcel = Model.find_by(address: address)
  lat = center_parcel.latitude
  lon = center_parcel.longitude

  DevelopmentMapIndex
    .filter(geo_distance: { distance: "#{params[:distance]}km", coordinates: { lat: lat, lon: lon } })
    .filter(query_for_details)
    .filter(query_for_details)
    .filter(query_for_details)
    .order(date_contract: :desc)s
    .types(input_2)
    .limit(10)
    .load()
    .objects()
end
```

