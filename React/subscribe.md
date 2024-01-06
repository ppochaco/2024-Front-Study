---
tags:
  - redux
---
# subscribe
: store의 내장 함수
- listener 함수를 파라미터로 넣어 호출하면, listener 함수가 action이 dispatch되어 상태가 업데이트 될 때마다 호출됨
```js
const listener = () => {
	console.log('상태 업데이트');
}
const unsubscribe = store.subscribe(listener);

unsubscribe();
```