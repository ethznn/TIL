# geokit usage

example codes

in model 

```ruby
# macros
# parcel.rb
class Parcel < ApplicationRecord
	...
  acts_as_mappable default_units: :kms,
    lat_column_name: :latitude, lng_column_name: :longitude
	...
end
```

in controller

```ruby
Parcel
	.within(1, origin: [parcel.latitude, parcel.longitude])
	.joins(:info).where(parcel_infos: { jimok: "대" }).each do |index|
    data << index.info
  end
```

