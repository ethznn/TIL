# Event Loop

[EventLoop 슬라이더](https://docs.google.com/presentation/d/1qzWMC6K5G4F0NwEuNSVVTme_BhL4tGzxU5DSTQWD2SA/edit#slide=id.p3)

많은 양의 라이브러리들, 도구들 그리고 당신의 개발을 더 쉽게 만들어주는 모든 것들로 인하여,

많은 프로그래머들이 가림막 아래 무언가가 어떻게 동작하는 지에 대한 이해가 없이 애플리케이션을 만들기 시작한다.

자바스크립트는 이 확실한 행동의 포스터소년이다.

가장 복잡한 언어이고 널리 퍼진 반면에 , 많은 개발자들을 높은 등급의 도구들을 사용하고 언어의 "나쁜 부분"들을 멀리하게끔 만들었다.

당신이 여전히 놀라운 애플리케이션들을 만들 수는 있겠지만  자바스크립트 폭풍우에 들어가는 것은 꽤 유익할 수 있다.

"이상한 부분"을 이해하는 것은 시니어 개발자들과 평균적인 단순한 코더들을 구분짓게 해주는   것이다.

그리고 자바스크립트 생태계가 끊임없이 변화하는 동안, 기본적인 것들은 다른 모든 도구들이 만들어 지는 것들의 꼭대기에 있는 것들이다.

그것들을 이해하는 것은 당신에게 더 넓은 지각과 개발 프로세스를 바라보는 관점의 변화를 준다.

### **이벤트루프는 무엇인가?**

당신은 자바스크립트가 싱글쓰레드 언어라는 것을 들어봤을 것이다.

당신은 콜스택과 이벤트큐와 같은 것들도 들어봤을 수도 있다.

많은 사람들은 이벤트루프가 자바스크립트가 콜백들과 프로미스들을 사용하게 허락한다고 알고있지만 더 많은 것들이 있다.

과한 세세한 부분들에 집중하는 대신, 우리는 자바스크립트 코드가 어떻게 실제로 실행되는지에 대한 높은 등급의 관점을 가질 것이다.

### **콜스택**

자바 스크립트는 단일의 콜스택이 있는데 그 안에서 이것은 우리가 현재 실행하고 있는 함수와 이후에 실행될 함수를 계속해서 파악하고 있다.

먼저 스택이 무엇인가?

스택은 약간의 제한이 있는 배열과 같은 자료구조이다.

당신은 뒤쪽에만 아이템들을 추가할 수 있고, 끝에 있는 아이템만 제거 할 수 있다.

또 다른 예는 쌓여있는 접시가 있다.

당신은 접시더미의 맨 위쪽에만 새 접시를 놓을 수 있고 또한 맨 위쪽에 있는 접시만 제거할 수 있다

당신이 함수를 실행할 때 그 함수가 콜스택에 추가된다. 그 다음 함수가 다른 함수를 호출 한다면 그 다른 함수는 콜스택의 맨 위쪽으로 올라갈 것이다.

당신이 콘솔에 에러메시지를 받을 때 실행 경로를 보여주는 긴 메시지를 받는다.

이것은 정확한 순간에 스택이 본 것이다. 이것이 그 정확한 순간에 스택이 본 것이다.

하지만 우리가 요청을 하거나 어떤 것에 타임아웃을 건다면?

이론적으로는 실행되는 동안 브라우저는 멈춰야한다. 그렇다면 콜스택은 계속 다음 함수를 진행할 수 있을까? 이론적으로는 그 브라우저가 실행되어지기 까지는 요청/타임아웃이  전체브라우저를 멈춰야 한다. 콜스택이 계속 되기 위해서.

그러나, 당신은 아무일도 일어나지 않는다는 것을 안다.

바로 이벤트테이블과 이벤트 큐 때문이다.

### **이벤트 테이블 & 이벤트 큐**

당신이 setTimeout 함수를 호출하거나 비동기관련 작업을 할 때마다 이벤트 테이블에 추가된다. 이것은 어떠한 이벤트 후에 어떤 함수가 호출되어야 한다고 알고 있는 자료구조이다.

이벤트가 발생(타임아웃, 클릭, 마우스움직임)하기만 하면 신호를 보낸다.

명심하라 이벤트 테이블은 함수들을 실행하지 않고 함수들을 콜스택에 이벤트테이블 마음대로 보내지도 않는다.

이것의 유일한 목적은 이벤트를 계속해서 파악하는 것이고 이벤트들을 이벤트 큐에 보내는 것이다. 함수들을 이벤트큐로 보내는 것이다.

이벤트 큐는 스택과 비슷한 자료구조다.

아이템을 맨 뒤쪽에 추가하지만 제거하는 것은 그것들의 가장 앞쪽부터 가능하다.

이벤트큐는 올바른 순서(order)를 저장하고 그 안에서 함수들은 실행된다.

이벤트테이블에서 함수들의 호출들을 받지만 콜스택으로 보낼필요가 있다 어떻게 할까? 이곳이 이벤트루프가 들어오는 곳이다.

### **이벤트루프**

우리는 드디어 악명높은 이벤트루프에 도달했다.

이것은 끊임없이 콜스택이 비었는지 확인하는 프로세스다.

이것을 시계인 것처럼 생각해보면, 이벤트루프가 째깍할 때마다 이건 콜스택을 보고, 그리고 콜스택이 비어있으면 이건 이벤트큐를 들여다본다. 매 초마다 콜스택이 비었는지 들여다보고 이벤트큐를 들여다본다.

이벤트큐 내에 기다리는 무언가 있다면 콜스택으로 옮겨지고 없다면 아무일도 일어나지 않는다.

여기 두 개의 흥미로운 케이스(상황들)가 있다.. 어떤 순서로 코드가 실행되리라 생각하는가?

어떤 사람들은 set timeout이 0초이기 때문에 즉시 실행될 것이라고 생각한다. 사실 이 특정한 예제에서 당신은 "first" 이전에 "second"를 먼저 보게 될 것이다.

자바스크립트는 setTimeout을 보고 말한다 "음, 나는 이것을 나의 이벤트테이블에 추가하고 실행을 계속하는게 좋겠군"

이것은 이벤트테이블을 거쳐 이벤트큐로 가고, 실행하기 위해서는  이벤트루프가 째깍할 때까지 기다릴 것이다.

### **악용**

이벤트루프의 행동에 관한 다른 흥미로운 예제는 재귀이다.

당신은 stack overflow 에러메시지를 본적 있는가?. 당신은 무한재귀를 만들 때 가끔 그것을 볼 것이다. 하지만 가끔 당신은 실제로 만들고 싶은 많은 수의 재귀 호출(recursive calls)을 가지고 있다.

당신의 코드 구조를 유지하며 터무니없는 양의 호출을 만들 수 있게 해주는 간단하지만 해킹스러운 제2의 해결책이 있다. 재귀 호출을 setTimeout 안에 감싸라. .

당신은 recursion() (이것이 당신의 메소드의 이름이라고 생각하라) 을 바로 호출하는 대신 setTimeout(() => recursion(), 0) 이렇게 호출한다.

이것은 stack overflow를 피할 것이다. 왜냐하면 그 호출들이 스택위에 바로 쌓이는 대신에 이벤트테이블과 큐를 거쳐갈 것이기 때문이다.

이 방법을 사용하지 않도록 노력하자, 하지만 이것은 자바스크립트의 동작에 대한 좋은 예제이다.

[EventLoop 눈으로 확인하기](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

### **결론**

더 많은 것들이 있고 이것은 루프와 전반적인 모든 것의 기본 설명이다.

나는 이것을 가능한 한 간단하게 설명하고 싶었지만 전반적인 과정을 조사하지 않고서는 이벤트루프가 무엇을 하는지 설명할 길이 없다.

이 외에 염두해야할 것은 이 설명은 V8 자바스크립트 엔진 맥락 안에서인것 이다.. 이 엔진은 크롬의 뒤에 있고 Node.js 에서 또한 사용된다.