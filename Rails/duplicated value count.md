# duplicated value count

example @region_count_hash

```ruby
@region_count_hash = Hash.new(0)
results.each { | v | @region_count_hash.store(v, @region_count_hash[v]+1) }
        
@region_count_hash = Hash.new(0)
	results.each do |region|
@region_count_hash[region] += 1
end

@region_count_hash = results.group_by(&:itself).map { |k,v| [k, v.count] }.to_h
```





http://forensic-proof.com/archives/3710