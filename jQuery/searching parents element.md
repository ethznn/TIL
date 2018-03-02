# Searching parents element

먼저 부모 요소를 선택하는 방법은 제이쿼리에도 몇 가지가 있습니다. 그 중에서 가장 자주 사용되는 방법으로 아래의 세가지 메소드가 대표적입니다.

```javascript
parent()
parents()
closest()
```

이와같이 위 메소드를 사용하면 간단하게 부모요소를 선택할 수 있습니다. 
그럼 어떻게 사용하고 각각의 메소드의 차이점은 무엇인지 알아봅니다.



parent() 메소드 알아보기

먼저 parent()는 해당 요소의 바로 위에 존재하는 하나의 부모 요소를 반환합니다. 위 예제에서는 p태그를 반환할 것입니다. parent()는 인접한 하나의 요소를 반환하는 것이 특징입니다.

#### parent() 예제소스 보기

아래예제는 현재 태그요소의 바로 위 부모 요소를 가져오는 예제입니다.

@ parent.html

```html
<div>
  <p>
    <span>How to find parent elements?</span>
  </p>
</div>
```

위 html은 예제에 사용된 html입니다. 아래는 스크립트 코드입니다.

@parent.js

```javascript
var tagName = $('span').parent().prop('tagName');
console.log(tagName);

// P를 출력함
```

위 코드를 실행하면 span 태그에 사용된 parent() 메소드로 

바로 위의 부모 요소인 p 태그를 가져와 출력합니다



#### parents() 메소드 알아보기

만약 하나가 아닌 

모든 부모 요소를 반환할 필요가 있다면 

parents()

 메소드를 사용합니다. 모든 요소를 반환한다는 점을 빼고는 parent()와 동일합니다. 아래 예제를 봐주세요.

@parents.js

```javascript
var parents = $('span').parents();

// parents 변수는 p와 div를 모두 가짐
```

이번에는 바로 위의 부모 요소인 p 뿐만 아니라 그 상위 부모인 div 역시 가지고 있습니다. 즉 모든 부모 요소를 가져와 함께 반환하는 것이 parent()와의 차이입니다.

#### closest() 메소드 알아보기

이번에 알아볼 closest() 메소드 역시 가장 많이 사용되는 메소드 중 하나입니다.  closest()는 

모든 부모 요소를 대상으로하여 원하는 요소만 선택자로 가져올 수 있습니다

. 예를들어 아래처럼 span 태그의 부모 요소중 div 태그를 가져올 수 있습니다.

@ closest.js

```javascript
var tagName = $('span').closest('div').prop('tagName');


// div를 출력
```

이처럼 부모 요소들 중에서 원하는 요소만 선택하여 가져올 수 있습니다.