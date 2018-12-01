Divide Files by R script

```R
R version 3.4.4 (2018-03-15) -- "Someone to Lean On"
Copyright (C) 2018 The R Foundation for Statistical Computing
Platform: x86_64-apple-darwin15.6.0 (64-bit)

R은 자유 소프트웨어이며, 어떠한 형태의 보증없이 배포됩니다.
또한, 일정한 조건하에서 이것을 재배포 할 수 있습니다.
배포와 관련된 상세한 내용은 'license()' 또는 'licence()'을 통하여 확인할 수 있습니다.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

'demo()'를 입력하신다면 몇가지 데모를 보실 수 있으며, 'help()'를 입력하시면 온라인 도움말을 이용하실 수 있습니다.
또한, 'help.start()'의 입력을 통하여 HTML 브라우저에 의한 도움말을 사용하실수 있습니다
R의 종료를 원하시면 'q()'을 입력해주세요.


> getwd() # PATH 찾기
[1] "/Users/pray/Documents/small_housing"
> list.files() # 파일들을 리스트화
[1] "11_lot_autoarchi_short.txt" "41_lot_autoarchi_short.txt"

list_file = list.files()
list_file1 "11_lot_autoarchi_short.txt"
install.packages('data.table')

--- 현재 세션에서 사용할 CRAN 미러를 선택해 주세요 ---

Secure CRAN mirrors

 1: 0-Cloud [https]                   2: Algeria [https]
 3: Australia (Canberra) [https]      4: Australia (Melbourne 1) [https]
 5: Australia (Melbourne 2) [https]   6: Australia (Perth) [https]
 7: Austria [https]                   8: Belgium (Ghent) [https]
 9: Brazil (PR) [https]              10: Brazil (RJ) [https]
11: Brazil (SP 1) [https]            12: Brazil (SP 2) [https]
13: Bulgaria [https]                 14: Chile 1 [https]
15: Chile 2 [https]                  16: China (Hong Kong) [https]
17: China (Guangzhou) [https]        18: China (Lanzhou) [https]
19: China (Shanghai 1) [https]       20: China (Shanghai 2) [https]
21: Colombia (Cali) [https]          22: Czech Republic [https]
23: Denmark [https]                  24: East Asia [https]
25: Ecuador (Cuenca) [https]         26: Ecuador (Quito) [https]
27: Estonia [https]                  28: France (Lyon 1) [https]
29: France (Lyon 2) [https]          30: France (Marseille) [https]
31: France (Montpellier) [https]     32: France (Paris 2) [https]
33: Germany (Erlangen) [https]       34: Germany (Göttingen) [https]
35: Germany (Münster) [https]        36: Greece [https]
37: Iceland [https]                  38: India [https]
39: Indonesia (Jakarta) [https]      40: Ireland [https]
41: Italy (Padua) [https]            42: Japan (Tokyo) [https]
43: Japan (Yonezawa) [https]         44: Korea (Busan) [https]
45: Korea (Seoul 1) [https]          46: Korea (Ulsan) [https]
47: Malaysia [https]                 48: Mexico (Mexico City) [https]
49: Norway [https]                   50: Philippines [https]
51: Serbia [https]                   52: Spain (A Coruña) [https]
53: Spain (Madrid) [https]           54: Sweden [https]
55: Switzerland [https]              56: Turkey (Denizli) [https]
57: Turkey (Mersin) [https]          58: UK (Bristol) [https]
59: UK (London 1) [https]            60: USA (CA 1) [https]
61: USA (IA) [https]                 62: USA (KS) [https]
63: USA (MI 1) [https]               64: USA (NY) [https]
65: USA (OR) [https]                 66: USA (TN) [https]
67: USA (TX 1) [https]               68: Vietnam [https]
69: (other mirrors)

선택: 1
URL 'https://cloud.r-project.org/bin/macosx/el-capitan/contrib/3.4/data.table_1.11.4.tgz'을 시도합니다.

...

> library(data.table)
data.table 1.11.4  Latest news: http://r-datatable.com
> d11 = fread(list_file[1])

> nrow(d11) / 5
[1] 1146270

for(i in 1:3) {
	if(i == 3) {
		t = d11[4000001:nrow(d11),]
		} else {
		t = d11[(1+(i-1)*2000000):(i*2000000),]
	}
	fwrite(t, paste0("11_lot_autoarchi_short_", i, ".txt"), row.names=F)
}
```