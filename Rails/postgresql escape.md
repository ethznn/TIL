# postgresql escape



"\n" 안먹는 문제 해결하려 데이터 삭제 후 다시 seed 하는 작업을 여러번 거쳤다.

 데이터베이스 안에 string 데이터가 어떻게 들어가있는지 확인한 후 

\\\n 으로 저장되어 \\\만 n을 제외하고 인식하고 있었음.



출력시에 .gsub("\\\n","\n") method를 추가해서 해결함.



```bash
$ rails c

$ puts "2종일반\\n주거지역".gsub("\\n","\n")
```

