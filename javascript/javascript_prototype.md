# 프로토타입 이해하기 prototype

자바스크립트는 프로토타입 기반 언어, 자바스크립트 개발을 하면 빠질 수 없는 것이 프로토타입

프로토 타입이 거의 자바스크립트 그 자체이기때문에 이해하는 것이 어렵고 개념도 복잡



### Prototype vs Class

class는 객체지향언어에서 빠질 수 없는 개념

하지만 자바스크립트도 객체지향언어!! 왜 중요하냐! 

**클라스라는 개념이 없다. 대신 프로토타입**이라는 것이 존재

클라스가 없어서 기본적으로 상속기능도 없다. 그래서 보통 프로토타입을 기반으로 상속을 흉내내도록 구현해 사용



자바스크립트에 클래스는 없지만 함수와 new를 통해 클래스를 비슷하게 사용

```javascript
function Person() {
  this.eyes = 2;
  this.nose = 1;
}
var kim  = new Person();
var park = new Person();

console.log(kim.eyes);  // => 2
console.log(kim.nose);  // => 1
console.log(park.eyes); // => 2
console.log(park.nose); // => 1
```

kim과 park은 eyes와 nose를 공통으로 가지고 있다. 메모리에는 eyes, nose 두개식 총 4개 할당 이를

```javascript
function Person() {}
Person.prototype.eyes = 2;
Person.prototype.nose = 1;
var kim  = new Person();
var park = new Person();
console.log(kim.eyes); // => 2
```

간단히 설명하면 Person.prototype이라는 빈 Object가 어딘가 존재하고, Person 함수로 생성된 객체(kim, park)들은 어딘가에 존재하는 Object에 들어있는 값을 모두 가져다 쓸 수 있다.

**즉, eyes와 nose를 어딘가 있는 빈공간(Person.prototype)에 넣어놓고 kim과 park이 공유해서 사용**

개발자가 사용하는 부분만 보면 이게 전부다 하지만 **왜??**



### Prototype Link와 Prototype Object

이 둘을 통틀어 Prototype이라고 부른다. 좀 안다는 것은 이 둘을 완벽히 이해하고 갖고 놀 수준이여야함!



#### Prototype Object

모든 객체(Object)의 조상은 함수(Function)

```javascript
function Person() {} // => 함수
var personObject = new Person(); // => 함수로 객체를 생성
```

personObject 객체는 Person이라는 함수로부터 파생된 객체

이렇게 언제나 객체는 함수로부터 시작!!

```javascript
var obj = {};
```

이 코드는 사실 다음과 같다.

```javascript
var obj = new Object();
```

위 코드 Object가 자바스크립트에서 기본적으로 제공하는 함수

Function, Array도 모두 함수로 정의 이게 **첫 번째 포인트**



Prototype Object와 무슨 상관이 있느냐! 함수가 정의될 때는 2가지 일이 동시에 발생

1. 해당 함수에 Constructor(생산자) 자격 부여

   * 생성자 자격이 부여되면 new를 통해 객체를 만들어 낼 수 있다. (함수만 new 키워드를 사용할 수 있음)

2. 해당 함수의 Prototype Object 생성 및 연결

   함수를 정의하면 함수만 생성되는 것이 아니라 Prototype Object도 같이 생성

![Prototype Object 생성 및 연결](https://cdn-images-1.medium.com/max/1600/1*PZe_YnLftVZwT1dNs1Iu0A.png)

​	prototype 이라는 속성을 통해 Prototype Object에 접근 할 수 있음.

​	Prototype Object는 일반적 객체와 같으며 기본적 속성으로 constructor와  _ _proto__ 을 가짐

​	constructor는 Porotype Object와 같이 생성되었던 함수를 가리킴

​	_ _proto__는 Prototype Link 

```javascript
function Person() {}
Person.prototype.eyes = 2;
Person.prototype.nose = 1;

var kim  = new Person();
var park = new Person():

console.log(kim.eyes); // => 2
```

Prototype Object는 일반적 객체로 속성 마음대로 추가/삭제 가능!!

kim과 park은 Person 함수를 통해 생성되었으니 Person.prototype을 참조할 수 있게 됩니다.



#### Prototype Link

prototype 속성은 함수만 가지고 있는데 _ _proto__ 속성은 모든 객체가 빠짐없이 가지고 있는 속성

객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리킨다. 예의 Person Prototype Object

![객체, 함수, Prototype Object의 관계](https://cdn-images-1.medium.com/max/1600/1*jMTxqTYDZGhykJQoimmb0A.png)



kim은 eyes를 직접 가지고 있지 않지만 eyes 속성을 찾아 상위 prototype을 탐색

최상위 Object의 Prototype Object까지 도달했는데도 못찾으면 undefined를 리턴

![프로토타입 체인, 최상위는 Object](https://cdn-images-1.medium.com/max/1600/1*mwPfPuTeiQiGoPmcAXB-Kg.png)

이렇게 _ _proto__ 속성을 통해 상위 프로토타입과 연결되어있는 형태를 **프로토타입 체인**이라고 한다.

![Object의 속성](https://cdn-images-1.medium.com/max/1600/1*VW4PFea8x7LQiHp3PI8Hrg.png)

이런 프로토타입 체인 구조 때문에 모든 객체는 **Object의 자식**이라고 불리고, 위에 보이는 Object Prototype Object에 있는 모든 속성을 사용할 수 있음.

