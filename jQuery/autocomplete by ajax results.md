# autocomplete by Ajax results in JQuery

html

```html
<div class="list no-hairlines-md">
  <ul>
    <li class="item-content item-input inline-label">
      <div class="item-inner">
        <div class="item-input-wrap">
          <input id="search-box" type="text" placeholder="주소" />
          <div id="suggesstion-box"></div>
        </div>
      </div>
    </li>
  </ul>
</div>
```

script

```javascript
$(document).ready(function () {
    var timeout = null;

    $("#search-box").bind('input propertychange', function () {
                
      var keyword = $('#search-box').val();

      var lot_number_settings = {
        "async": true,
        "crossDomain": true,
        "url": "api_url/query="+keyword,
        "method": "POST",
        "headers": {
          "Authorization": "<%=Rails.application.credentials.key%>",
          'Content-Type' : 'application/x-www-form-urlencoded; charset=UTF-8'
        }
      }
      
      clearTimeout(timeout);
      timeout = setTimeout(function () {

        if (keyword.length >= 2) {
          $("#suggesstion-box").empty();
        
          $.ajax(lot_number_settings).done(function (response) {
            // console.log(response);
            while (is_end == "false") {
              for (var i = 0; i < response["documents"].length; i++) {
                response_address = response["documents"][i]["address_name"]
                response_lat = response["documents"][i]["x"]
                response_lon = response["documents"][i]["y"]
                if (response["documents"][i] != null) {
                  $("#suggesstion-box").append('<tr><td>'+response_address+'</td></tr>');
                }
              }
              is_end = response["meta"]["is_end"]
            }
          });
        } else {
          $("#suggesstion-box").empty();
        }
      }, 300);
    });
  });
```

