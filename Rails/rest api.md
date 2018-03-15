```ruby
		require 'uri'
    require 'net/http'

    url = URI("http://ip/api/sensor/data/list")

    http = Net::HTTP.new(url.host, url.port)

    request = Net::HTTP::Post.new(url)
    request["content-type"] = 'multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW'
    # request["Content-Type"] = 'application/json'
    request["Cache-Control"] = 'no-cache'
    request["Postman-Token"] = '??'
    request.body = "------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"data\"\r\n\r\n{ \"key\":\"??\",\"data\": {\"userId\":\"treeplanet\",\n\"sensorTypeCd\":\"02\",\n\"startDate\":\"2016-11-04\",\n\"endDate\":\"2016-11-04\",\n\"curPage\":\"1\",\n\"pageListSize\":\"10\",\n\"tfHubSensorBmTypeIds\": [0, 1]}}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--"

    response = http.request(request)
    puts response.read_body
```

