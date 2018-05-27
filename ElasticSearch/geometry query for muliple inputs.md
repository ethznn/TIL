# geometry query for multiple inputs

geometry query 를 사용하면 한 번의 쿼리로

여러 중점의 위치를 받아서 각 중점 위치마다 거리내의 결과물을 걸러내는 필터를 만들 수 있다. 중복 값 제외



helper code for practice // rails 5.2 chwey 이용

```ruby
def query_for_distance_by_input(inputs)
  coordinate = []
  inputs.each do |index|
    coordinate << [index.latitude, index.longitude]
  end

  distance = 
  if params[:distance].present?
    params[:distance]
  else
		default값
  end

  multi_coordinate_query = []

  coordinate.each do |index|
    multi_coordinate_query << 
    {
      geo_distance: { 
        distance: "#{distance}km", 
        coordinates: {
          lat: index[0], 
          lon: index[1]
          }
        }
      }
  end

  hash = {
    bool: { 
      should: multi_coordinate_query # bool should(OR), must(and), must_not(except)
      }
    }
end
```

bool 값을 활용하면 더욱 강력한 필터 기능이 가능하다.

* should (OR)
* must (AND)
* must_not (EXCEPT)