pluck & to_h
===

Model.pluck(:ex_key1, :ex_key2).to_h
---

### => [ex_value1, ex_value2].to_h

### => {ex_key1 => ex_value1, ex_key2 => ex_value2}

### model에서 필요한 key값에 해당하는 value를 뽑아서 hash 로 저장
