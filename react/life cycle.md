# life cycle

컴포넌트가 브라우저에 나타나기 전, 후에 호출되는 API

## constructor
```js
constructor(props) {
  super(props);
}
```
이전에 언급 한적이 있죠? 이 부분은 컴포넌트 생성자 함수. 컴포넌트가 새로 만들어질 때마다 호출

## componentWillMount
```js
componentWillMount() {

}
```
이 API 는 컴포넌트가 여러분의 화면에 나가나기 직전에 호출되는 API, 이 API 에 대해선 별로 신경쓰지 않아도 됩니다. 원래는 주로 브라우저가 아닌 환경에서 (서버사이드)도 호출하는 용도로 사용했었는데, 이 API 가 더 이상 필요하지 않게 되어 리액트 v16.3 에서는 해당 API 가 deprecated 되었으니, 아, 옛날엔 이러한 API가 사용됐었구나.. 하고 알아만 두시면 됩니다. v16.3 이후부터는 UNSAFE_componentWillMount() 라는 이름으로 사용됩니다. 기존에 이 API 에서 하던 것들은 위에 있는 constructor 와 아래에서 다뤄볼 componentDidMount 에서 충분히 처리 할 수 있습니다.

## componentDidMount
```js
componentDidMount() {
  // 외부 라이브러리 연동: D3, masonry, etc
  // 컴포넌트에서 필요한 데이터 요청: Ajax, GraphQL, etc
  // DOM 에 관련된 작업: 스크롤 설정, 크기 읽어오기 등
}
```
이 API 는 여러분의 컴포넌트가 화면에 나타나게 됐을 때 호출됩니다. 여기선 주로 D3, masonry 처럼 DOM 을 사용해야하는 외부 라이브러리 연동을 하거나, 해당 컴포넌트에서 필요로하는 데이터를 요청하기 위해 axios, fetch 등을 통하여 ajax 요청을 하거나, DOM 의 속성을 읽거나 직접 변경하는 작업을 진행합니다.

컴포넌트 업데이트  
컴포넌트가 업데이트는 props 의 변화, 그리고 state 의 변화에 따라 결정됩니다. 업데이트가 되기 전과 그리고 된 후에 어떠한 API 가 호출 되는지 살펴볼까요?

## componentWillReceiveProps
```js
componentWillReceiveProps(nextProps) {
  // this.props 는 아직 바뀌지 않은 상태
}
```
이 API 는 컴포넌트가 새로운 props 를 받게됐을 때 호출됩니다. 이 안에서는 주로, state 가 props 에 따라 변해야 하는 로직을 작성합니다. 새로 받게될 props 는 nextProps 로 조회 할 수 있으며, 이 때 this.props 를 조회하면 업데이트 되기 전의 API 이니 참고하세요. 이 API 또한 v16.3 부터 deprecate 됩니다. v16.3 부터는 UNSAFE_componentWillReceiveProps() 라는 이름으로 사용됩니다. 그리고, 이 기능은 상황에 따라 새로운 API getDerivedStateFromProps 로 대체 될 수도 있습니다.

## [NEW] static getDerivedStateFromProps()
이 함수는, v16.3 이후에 만들어진 라이프사이클 API 인데요, 이 API 는 props 로 받아온 값을 state 로 동기화 하는 작업을 해줘야 하는 경우에 사용됩니다.
```js
static getDerivedStateFromProps(nextProps, prevState) {
  // 여기서는 setState 를 하는 것이 아니라
  // 특정 props 가 바뀔 때 설정하고 설정하고 싶은 state 값을 리턴하는 형태로
  // 사용됩니다.
  /*
  if (nextProps.value !== prevState.value) {
    return { value: nextProps.value };
  }
  return null; // null 을 리턴하면 따로 업데이트 할 것은 없다라는 의미
  */
}
```
## shouldComponentUpdate
```js
shouldComponentUpdate(nextProps, nextState) {
  // return false 하면 업데이트를 안함
  // return this.props.checked !== nextProps.checked
  return true;
}
```
이 API 는 컴포넌트를 최적화하는 작업에서 매우 유용하게 사용됩니다. 우리가 저번에 배웠을 떄, 리액트에서는 변화가 발생하는 부분만 업데이트를 해줘서 성능이 꽤 잘 나온다고 했었지요? 하지만, 변화가 발생한 부분만 감지해내기 위해서는 Virtual DOM 에 한번 그려줘야합니다.

즉, 현재 컴포넌트의 상태가 업데이트되지 않아도, 부모 컴포넌트가 리렌더링되면, 자식 컴포넌트들도 렌더링 됩니다. 여기서 “렌더링” 된다는건, render() 함수가 호출된다는 의미입니다.

변화가 없으면 물론 DOM 조작은 하지 않게 됩니다. 그저 Virutal DOM 에만 렌더링 할 뿐이죠. 이 작업은 그렇게 부하가 많은 작업은 아니지만, 컴포넌트가 무수히 많이 렌더링된다면 얘기가 조금 달라집니다. CPU 자원을 어느정도 사용하고 있는것은 사실이니까요.

쓸대없이 낭비되고 있는 이 CPU 처리량을 줄여주기 위해서 우리는 Virtual DOM 에 리렌더링 하는것도,불필요할경우엔 방지하기 위해서 shouldComponentUpdate 를 작성합니다.

이 함수는 기본적으로 true 를 반환합니다. 우리가 따로 작성을 해주어서 조건에 따라 false 를 반환하면 해당 조건에는 render 함수를 호출하지 않습니다.

## componentWillUpdate
```js
componentWillUpdate(nextProps, nextState) {

}
```
이 API는 shouldComponentUpdate 에서 true 를 반환했을때만 호출됩니다. 만약에 false 를 반환했었다면 이 함수는 호출되지 않습니다. 여기선 주로 애니메이션 효과를 초기화하거나, 이벤트 리스너를 없애는 작업을 합니다. 이 함수가 호출되고난 다음에는, render() 가 호출됩니다. 이 API 또한 v16.3 이후 deprecate 됩니다. 기존의 기능은 getSnapshotBeforeUpdate 로 대체 될 수 있습니다.

## [NEW] getSnapshotBeforeUpdate()
이 API 가 발생하는 시점은 다음과 같습니다.
```js
render()
getSnapshotBeforeUpdate()
```
실제 DOM 에 변화 발생
componentDidUpdate
이 API를 통해서, DOM 변화가 일어나기 직전의 DOM 상태를 가져오고, 여기서 리턴하는 값은 componentDidUpdate 에서 3번째 파라미터로 받아올 수 있게 됩니다.

예시코드:
```js
getSnapshotBeforeUpdate(prevProps, prevState) {
  // DOM 업데이트가 일어나기 직전의 시점입니다.
  // 새 데이터가 상단에 추가되어도 스크롤바를 유지해보겠습니다.
  // scrollHeight 는 전 후를 비교해서 스크롤 위치를 설정하기 위함이고,
  // scrollTop 은, 이 기능이 크롬에 이미 구현이 되어있는데, 
  // 이미 구현이 되어있다면 처리하지 않도록 하기 위함입니다.
  if (prevState.array !== this.state.array) {
    const {
      scrollTop, scrollHeight
    } = this.list;

    // 여기서 반환 하는 값은 componentDidMount 에서 snapshot 값으로 받아올 수 있습니다.
    return {
      scrollTop, scrollHeight
    };
  }
}

componentDidUpdate(prevProps, prevState, snapshot) {
  if (snapshot) {
    const { scrollTop } = this.list;
    if (scrollTop !== snapshot.scrollTop) return; // 기능이 이미 구현되어있다면 처리하지 않습니다.
    const diff = this.list.scrollHeight - snapshot.scrollHeight;
    this.list.scrollTop += diff;
  }
}
```

## componentDidUpdate
```js
componentDidUpdate(prevProps, prevState, snapshot) {

}
```
이 API는 컴포넌트에서 render() 를 호출하고난 다음에 발생하게 됩니다. 이 시점에선 this.props 와 this.state 가 바뀌어있습니다. 그리고 파라미터를 통해 이전의 값인 prevProps 와 prevState 를 조회 할 수 있습니다. 그리고, getSnapshotBeforeUpdate 에서 반환한 snapshot 값은 세번째 값으로 받아옵니다.

컴포넌트 제거
컴포넌트가 더 이상 필요하지 않게 되면 단 하나의 API 가 호출됩니다:

## componentWillUnmount
```js
componentWillUnmount() {
  // 이벤트, setTimeout, 외부 라이브러리 인스턴스 제거
}
```
여기서는 주로 등록했었던 이벤트를 제거하고, 만약에 setTimeout 을 걸은것이 있다면 clearTimeout 을 통하여 제거를 합니다. 추가적으로, 외부 라이브러리를 사용한게 있고 해당 라이브러리에 dispose 기능이 있다면 여기서 호출해주시면 됩니다.