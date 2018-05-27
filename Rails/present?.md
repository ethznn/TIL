# present?

find_by로 값을 찾지 못하면 nil을 return하는 반면 

where 로 model의 row를 찾지 못했을때, nil이 아닌 [] 값을 리턴하는 rails의 특징 때문에

if else 문을 쓸때,  어려움이 있을 때가 있다. 

이때 .present? 를 사용하자.

```ruby
result = Model.where(:id => "사용자의 input 값")

if result.present?	# model의 where로 값을 못 찾았을때(result=[]일때) false를 return
  puts "hello world"
end
```

