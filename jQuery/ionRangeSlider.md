## ion.RangeSlider

#### html

```html
<input type="number" name="slider_name" id="slider_id" class="">
```



#### jQuery

```javascript
$("#sider_id").ionRangeSlider({
  type: "double",
  grid: true,
  min: min_value,
  max: max_value,
  from: start_point,
  to: end_point,
  max_postfix: "+", // 최대값을 제한 두지 않음
  force_edges: true,
  onFinish: function(data) { // 슬라이더 조절 시에 변경 내용에 따라 실행할 액션들
    actions() || funtions(); 
  }
});
```

#### functions

```javascript
// update ex) from & to 
$("#sider_id").data("ionRangeSlider").update({
  from: change_start_point,
  to: change_end_point
});

// reset
$("#sider_id").data("ionRangeSlider").reset();
```



#### style

```scss
// ex styling
.irs {
    height: 40px;
    margin:20px 0;
}
.irs-with-grid {
    height: 60px;
}
.irs-line {
    height: 10px; top: 33px;
    background: #EEE;
    background: linear-gradient(to bottom, #DDD -50%, #FFF 150%); /* W3C */
    border:1px SOLID #CCC;
    border-radius: 16px;
}
    .irs-line-left {
        height: 8px;
    }
    .irs-line-mid {
        height: 8px;
    }
    .irs-line-right {
        height: 8px;
    }

.irs-diapason {
    height: 10px; top: 33px;
    border:1px SOLID #428bca;
    background: #428bca;
    background: linear-gradient(to top, rgba(66,139,202,1) 0%,rgba(127,195,232,1) 100%); /* W3C */
}

.irs-slider {
    top:25px;
    width: 27px; height: 27px;
    border:1px SOLID #AAA;
    background: #DDD;
    background: linear-gradient(to bottom, rgba(255,255,255,1) 0%,rgba(220,220,220,1) 20%,rgba(255,255,255,1) 100%); /* W3C */
    border-radius: 27px;
    box-shadow: 1px 1px 3px rgba(0,0,0,0.3);
    cursor:pointer;
}

#irs-active-slider, .irs-slider:hover {
    background: #FFF;
}

.irs-min, .irs-max {
    color: #333;
    font-size: 12px; line-height: 1.333;
    text-shadow: none;
    top: 0;
    padding: 1px 5px;
    background: rgba(0,0,0,0.1);
    border-radius: 3px;
}

.lt-ie9 .irs-min, .lt-ie9 .irs-max {
    background: #ccc;
}

.irs-from, .irs-to, .irs-single {
    color: #fff;
    font-size: 14px; line-height: 1.333;
    text-shadow: none;
    padding: 1px 5px;
    background: #428bca;
    border-radius: 3px;
}
.lt-ie9 .irs-from, .lt-ie9 .irs-to, .lt-ie9 .irs-single {
    background: #999;
}

.irs-grid-pol {
    background: #99a4ac;
}

.irs-grid-text {
    color: #99a4ac;
}

.irs-disabled {
}
```



참고 

- [ion.RangeSlider Interactions Demo Page](http://ionden.com/a/plugins/ion.rangeslider/demo_interactions.html)
- [custom css](https://gist.github.com/guybowden/90f42413649148df7632)