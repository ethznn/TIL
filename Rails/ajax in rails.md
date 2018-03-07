# ajax in Rails

먼저 ajax 요청 보내기

```ruby
<%= link_to my_example_path(@sample, sample_id: id, sample_name: name, sample_address: address), remote: true do %>
```

