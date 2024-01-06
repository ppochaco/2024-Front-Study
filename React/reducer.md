---
tags:
  - redux
---
# reducer
: 변화를 일으키는 함수
- 특징
	- action을 만들면 reducer가 현재 상태와 액션 객체를 파라미터로 받아와 새로운 상태 반환

```js
const initialState = {
	counter: 1
};
function reducer(state = initialState, action) {
	switch (action.type) {
		case INCREMENT:
		return {
			counter: state.counter + 1
		}
	}
}
```