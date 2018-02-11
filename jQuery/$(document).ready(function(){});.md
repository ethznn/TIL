#### $(document).ready(function() {});

##### $(document).ready(function() {});

 => JavaScript의 onload와 같은 기능.

 모든 html 이 화면이 뿌려지고 나서 저 ready 안에 서술된 이벤트들이 동작 준비.

$(function() {}); 같은 표현



html 소스 흐름은 위에서 아래로 순차적으로 진행!!

but 위의 $(document).ready(function() {});를 쓰면 그 { } 안의 code를 제일 나중에 읽어온다.