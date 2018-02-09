#### 버튼으로 체크박스 제어하기 

체크박스 하나라도 체크 되어있을떄 클릭하면 모두 다 해제

아예 안 되어있다면 모두 체크

```javascript
$(document).on("행돌(click,change)", "div,button[name='name값']",  function(e) {
	e.preventDefault();
    var checked_count = $("div,button[name='name값']:checked").length
    if (checked_count == 0) {
      $("div,button[name='name값']").prop("checked", true);
    } else {
      $("div,button[name='name값']").prop("checked", false);
    }
});
```

